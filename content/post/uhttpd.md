---
date: 2023-02-04
title: openwrt source code reading - uhttpd
weight: 10
---

# lua support

lua support is done via the uhttp plugin system.

During 

```
struct uhttpd_plugin uhttpd_plugin = {
    .init = lua_plugin_init,
};
```

# higher level construction

Each plugin has a struct `uhttpd_plugin`, which has the .init function which is
called on the plugins initialization during uhttpd start up.


```
void uh_plugin_post_init(void)
{
    struct uhttpd_plugin *p;

    list_for_each_entry(p, &plugins, list)
        if (p->post_init)
            p->post_init();
}
```

This linked-list `plugins` is constructed in `uh_plugin_init(const char *name)`
function calls:

```
 if (!list_empty(&conf.lua_prefix) && uh_plugin_init("uhttpd_lua.so"))
     return 1;
```

# lower level information

## lua_plugin_init

This function will initialize each lua_prefx (corresponding to and url prefix)
using uh_lua_state_init function call.

Then it will use the ops (the uhttp functions) to register the dispatch struct.

```
static int lua_plugin_init(const struct uhttpd_ops *o, struct config *c)
{
	struct lua_prefix *p;

	ops = o;
	_conf = c;

	list_for_each_entry(p, &conf.lua_prefix, list)
		uh_lua_state_init(p);

	ops->dispatch_add(&lua_dispatch);
	return 0;
}
```

## dispatch_handler

The dispatch handler is where the url is matched (by check_url function calls)
and handled (by handle_requet function calls)

```
static struct dispatch_handler lua_dispatch = {
	.script = true,
	.check_url = check_lua_url,
	.handle_request = lua_handle_request,
};
```


The dispatch is added to following global list.

```
static LIST_HEAD(dispatch_handlers);
```


Refer to following function in file.c

```
void uh_dispatch_add(struct dispatch_handler *d)
{
	list_add_tail(&d->list, &dispatch_handlers);
}
```


# how dispatch is used

```
uh_handle_request -> __handle_file_request ->
dispatch_find -> check_url
```

In `__handle_file_request`, if the dispatch handler is found, it will invoke the
handler :

```
	if (is_error_handler) {
		d = dispatch_find(url, NULL);

		if (d) {
			uh_invoke_handler(cl, d, url, NULL);

			return true;
		}
	}

```

# How lua handle each request

In case of lua, all the request has same url prefix, so for the each specific
lua url, it will try to match the exact lua script path to handle the request.

```
static void lua_handle_request(struct client *cl, char *url, struct path_info *pi)
{
	struct lua_prefix *p;
	static struct path_info _pi;

	list_for_each_entry(p, &conf.lua_prefix, list) {
		if (!ops->path_match(p->prefix, url))
			continue;

		pi = &_pi;
		pi->name = p->prefix;
		pi->phys = p->handler;

		_L = p->ctx;

		if (!ops->create_process(cl, pi, url, lua_main)) {
			ops->client_error(cl, 500, "Internal Server Error",
			                  "Failed to create CGI process: %s",
			                  strerror(errno));
		}

		return;
	}

	ops->client_error(cl, 500, "Internal Server Error",
	                  "Failed to lookup matching handler");
}
```


# cgi support

cig support is a built-in feature, which means it is not supported via plugin
system. But nevermind, it is similar with a plugin, it just needs to register a
`dispatch_handler`.

cig handler is very similar to lua handler: it will create a sub-process and
run the `cgi_main` in that process, just like in lua handler, `lua_main` is
finally called.

```
static void cgi_handle_request(struct client *cl, char *url, struct path_info *pi)
{
	unsigned int mode = S_IFREG | S_IXOTH;
	char *escaped_url;

	if (!pi->ip && !((pi->stat.st_mode & mode) == mode)) {
		escaped_url = uh_htmlescape(url);

		uh_client_error(cl, 403, "Forbidden",
				"You don't have permission to access %s on this server.",
				escaped_url ? escaped_url : "the url");

		if (escaped_url)
			free(escaped_url);

		return;
	}

	if (!uh_create_process(cl, pi, url, cgi_main)) {
		uh_client_error(cl, 500, "Internal Server Error",
				"Failed to create CGI process: %s", strerror(errno));
		return;
	}

	return;
}
```

# ubus support

The ubus support is done via plugin  however the handling of request is not via
a separate process. The reasons why lua and cgi support needs a separate are
because that the lua and cgi needs standalone environment. 


# ucode support

Similar to cgi and lua support, ucode is supported via plugin and it needs a
process to run the handler.

```
static void
ucode_handle_request(struct client *cl, char *url, struct path_info *pi)
{
....
		if (!ops->create_process(cl, pi, url, ucode_main)) {
			ops->client_error(cl, 500, "Internal Server Error",
			                  "Failed to create CGI process: %s",
			                  strerror(errno));
....
}
```


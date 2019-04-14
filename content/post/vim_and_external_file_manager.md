---
date: 2018-02-14
title: Using external file explorer with vim
weight: 10
---

# using external file explorer

http://midnight-commander.org/

Then config it to use external editor vim

For open all files in one vim, one may open a vim server

```bash
vim --servername server_name_here
```

then open in other location
```bash
vim --servername server_name_here --remote file_name_here
```

One can get all lines:
```
vim --servername server_name_here --remote-expr "getline(1,'$')"
```

One can issue commands:
```
vim --servername server_name_here  --remote-send "<ESC>"
```

Through --remote-expr and --remote-send, one could almost totally control the vim server.



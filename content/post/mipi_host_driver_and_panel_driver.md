---
date: 2020-04-05
title: MIPI host driver and panel driver
weight: 10
---

# Introduction

The relationship of host driver and panel driver is introduced here. The design
is a very good example of a design pattern: aggregation or facade. Declaimer:
this document is not a complete reference document for the drivers, but just a
notes to understand from one perspective.

# Concepts

## MIPI DSI Host Driver

What is the host? The host basically means the SOC itself. The MIPI Host driver
is in e.g.:

```
drivers/gpu/drm/rockchip/dw-mipi-dsi.c  
```

It will controls the registers in SOC in order to connect to a panel.


## MIPI Panel Driver

What is the panel? It basically means the Panel, which could be controlled BY
SoC via communication channels, for example in case of MIPI DSI, the host could
control the panel via D0 vis DSI commands.

## Connector

A connector is an aggregate `concept`. The separation of `Host driver` and
`Panel driver` is for decoupling the two logic parts. However it might be
confusing for the client of host driver and panel driver, as the client just
how to `show` or `display`. A `connector` is an aggregation of two logical
parts and expose only one interface to the client.

# How these two drivers are bound together

So how these two driver interact with each other? This is no magic here, it
is done via callbacks/registrations. Specifically, it is done via `component`.

Refer to
[link](https://www.kernel.org/doc/html/latest/driver-api/component.html) for
the details of concept of `component`:

In short, component allows to aggregate drivers into an aggregated drivers, in
our case, it helps to aggregate the MIPI DSI host driver and sub-dev panel
driver.

In a bit more details, the host driver register its self as a component, once
it is done, the `bind` callback will be called by the `component` system. In
this `bind` callback, the host driver will try to do the binding with its
panel driver.


## More details

The design of `component` is quite simple but powerful. It allows the host
driver and panel driver to become more static, but move the dynamic part under
the cover of `component` helper system.

### probing of host driver

During probe of MIPI DSI Host driver, following component (ops) is registered:


```
ret = component_add(dev, &dw_mipi_dsi_ops);
```

The host DSI ops has two callback functions: `bind` and `unbind`.
```
static const struct component_ops dw_mipi_dsi_ops = {
    .bind   = dw_mipi_dsi_bind,
    .unbind = dw_mipi_dsi_unbind,
};
```

### Component calling `bind` callback

In this binding function it will check if the panel driver is available. If
not, it will return `EPROBE_DEFER` to indicate the `component` system that
'please call me later'. The component system will call it again a bit later.

Following function is called to check if the panel is available:
```
dsi->panel = of_drm_find_panel(dsi->client);
```

`of_drm_find_panel` is a helper function to check if the panel is already added
by function `drm_panel_add`, which is called by panel driver during probing.

Refer to:
```
drivers/gpu/drm/drm_panel.c 
```

### probing of panel driver
Refer to following source code for for the calling of `drm_panel_add` by the
panel driver:

```
drivers/gpu/drm/panel/panel-simple.c
```

### continuing of probing of Host driver

Once the host driver (in binding function) find the correct panel, it will
continue its logic, it will other factors and it may tell to be called later
again by return `-EPROBE_DEFER`.

Refer to a specific host driver:
```
drivers/gpu/drm/rockchip/dw-mipi-dsi.c
```

### `connector`

#### `get_modes`
As we understand earlier, `connector` will expose some functions. One of them
is `get_modes` function, as shown in below:

```
static const struct drm_connector_helper_funcs
dw_mipi_dsi_connector_helper_funcs = {
    .get_modes = dw_mipi_dsi_connector_get_modes,
    .best_encoder = dw_mipi_dsi_connector_best_encoder,
};
```

It will call the panel's driver's function eventually:
```
int panel_simple_get_modes(struct drm_panel *panel)
```

##### `encoder`

Another part of a `connector` is the encoder, the encoder has several callbacks
to be used by the client:

```
static const struct drm_encoder_helper_funcs
dw_mipi_dsi_encoder_helper_funcs = {
    .loader_protect = dw_mipi_dsi_encoder_loader_protect,
    .mode_set = dw_mipi_dsi_encoder_mode_set,
    .enable = dw_mipi_dsi_encoder_enable,
    .disable = dw_mipi_dsi_encoder_disable,
    .atomic_check = dw_mipi_dsi_encoder_atomic_check,
};
```

# Initialization Steps

- DSI Encoder Enabled (Set the Lane/Clock)
- Set the VoP routing to DSI
- Set the DSI to command mode
- Toggle Panel's Reset pin and Enable pin to activate panel (for receiving commands)
- After some delay, send init-sequence commands
- Set the DSI to video mode (so DSI could output video packets)
- Enable Panel's backlight


# Summary

The `aggregation` design patten allows logic separation of host driver and
panel driver. While `facade` design patten allows an unified interface to the
client.

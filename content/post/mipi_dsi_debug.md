---
date: 2020-04-05
title: MIPI DSI panel debugging
weight: 10
---

# Issue

The MIPI DSI panel does not work, it is totally black, showing nothing.

# The logic under the hood

Refer to  the [document](mipi_host_driver_and_panel_driver) for how it works in
the MIPI DSI host driver and panel driver.

After adding a printing in the `connector` callback functions, it is found that
following function is not called during booting up.

```
void dw_mipi_dsi_encoder_enable(struct drm_encoder *encoder)
```

But it is called every late:

```
[   43.284590] encoder: dw_mipi_dsi_encoder_enable: debuga start
[   43.284663] dw_mipi_dsi_encoder_enable: lane_rate 433333333
[   43.284769] dw-mipi-dsi ff960000.dsi: final DSI-Link bandwidth: 432 x 4 bps
[   43.284835] dw_mipi_dsi_encoder_enable: dw_mipi_dsi_vop_routing
[   43.285299] dw_mipi_dsi_encoder_enable: dw_mipi_dsi_pre_enable
[   43.288170] encoder: dw_mipi_dsi_encoder_loader_protect: debuga start
[   43.288304] encoder: dw_mipi_dsi_loader_protect: debuga start
[   43.288358] encoder: dw_mipi_dsi_encoder_disable: debuga start
```

# the client of `host driver` and `panel driver`

```
/home/pandy/rk3368_9/kernel/drivers/gpu/drm/rockchip/rockchip_drm_drv.c:1971
```

```
static const struct of_device_id rockchip_drm_dt_ids[] = {
    { .compatible = "rockchip,display-subsystem", },
    { /* sentinel */ },
};
```

```
/**
 * struct drm_encoder_helper_funcs - helper operations for encoders
 * @dpms: set power state
 * @save: save connector state
 * @restore: restore connector state
 * @mode_fixup: try to fixup proposed mode for this connector
 * @prepare: part of the disable sequence, called before the CRTC modeset
 * @commit: called after the CRTC modeset
 * @mode_set: set this mode, optional for atomic helpers
 * @get_crtc: return CRTC that the encoder is currently attached to
 * @detect: connection status detection
 * @disable: disable encoder when not in use (overrides DPMS off)
 * @enable: enable encoder
 * @atomic_check: check for validity of an atomic update
 *
 * The helper operations are called by the mid-layer CRTC helper.
 *
 * Note that with atomic helpers @dpms, @prepare and @commit hooks are
 * deprecated. Used @enable and @disable instead exclusively.
 *
 * With legacy crtc helpers there's a big semantic difference between @disable
 * and the other hooks: @disable also needs to release any resources acquired in
 * @mode_set (like shared PLLs).
 */
struct drm_encoder_helper_funcs {
    int (*loader_protect)(struct drm_encoder *encoder, bool on);
    void (*dpms)(struct drm_encoder *encoder, int mode);
    void (*save)(struct drm_encoder *encoder);
    void (*restore)(struct drm_encoder *encoder);

    bool (*mode_fixup)(struct drm_encoder *encoder,
               const struct drm_display_mode *mode,
               struct drm_display_mode *adjusted_mode);
    void (*prepare)(struct drm_encoder *encoder);
    void (*commit)(struct drm_encoder *encoder);
    void (*mode_set)(struct drm_encoder *encoder,
             struct drm_display_mode *mode,
             struct drm_display_mode *adjusted_mode);
    struct drm_crtc *(*get_crtc)(struct drm_encoder *encoder);
    /* detect for DAC style encoders */
    enum drm_connector_status (*detect)(struct drm_encoder *encoder,
                        struct drm_connector *connector);
    void (*disable)(struct drm_encoder *encoder);

    void (*enable)(struct drm_encoder *encoder);

    /* atomic helpers */
    int (*atomic_check)(struct drm_encoder *encoder,
                struct drm_crtc_state *crtc_state,
                struct drm_connector_state *conn_state);
};
```
# Analysis of the log


```
>> encoder as part of dsi host driver:
1   [   52.717775] encoder: dw_mipi_dsi_encoder_atomic_check: debuga start
2   [   52.718275] encoder: dw_mipi_dsi_encoder_mode_set: debuga start

>> lcdc controller (vop) setting
3   [   52.718411] rockchip-vop ff930000.vop: [drm:vop_crtc_enable] Update mode to 800x1280p57, type: 16
4   [   52.719746] CPU: 0 PID: 269 Comm: composer@2.1-se Not tainted 4.4.167 #16
5   [   52.719818] Hardware name: Rockchip rk3368 xkp avb board (DT)

>> called by iotrl that to enable encoder
6   [   52.719852] Call trace:
7   [   52.719915] [<ffffff8008089f70>] dump_backtrace+0x0/0x1ec
8   [   52.719999] [<ffffff800808a170>] show_stack+0x14/0x1c
9   [   52.720051] [<ffffff80083ad558>] dump_stack+0x94/0xbc
10  [   52.720100] [<ffffff80084bae68>] dw_mipi_dsi_encoder_enable+0xd0/0x388
11  [   52.720155] [<ffffff80084834a0>] drm_atomic_helper_commit_modeset_enables+0xdc/0x188
12  [   52.720222] [<ffffff80084c12b8>] rockchip_atomic_commit_complete+0x48/0xe8
13  [   52.720272] [<ffffff80084c1540>] rockchip_drm_atomic_commit+0x1e8/0x210
14  [   52.720318] [<ffffff80084a9664>] drm_atomic_commit+0x64/0x70
15  [   52.720381] [<ffffff80084aab50>] drm_mode_atomic_ioctl+0x5d0/0x6a0
16  [   52.720426] [<ffffff800848e394>] drm_ioctl+0x2e4/0x400
17  [   52.720476] [<ffffff80081d20fc>] do_vfs_ioctl+0xa4/0x7d8
18  [   52.720523] [<ffffff80081d288c>] SyS_ioctl+0x5c/0x8c
19  [   52.720567] [<ffffff80080832f0>] el0_svc_naked+0x24/0x28
20  [   52.720617] encoder: dw_mipi_dsi_encoder_enable: debuga start
21  [   52.720649] dw_mipi_dsi_encoder_enable: lane_rate 433333333
22  [   52.720711] dw-mipi-dsi ff960000.dsi: final DSI-Link bandwidth: 432 x 4 Mbps

>> enable the routing voip -> dsi
23  [   52.720744] dw_mipi_dsi_encoder_enable: dw_mipi_dsi_vop_routing

>> reset dsi interface, power on dphy, power on host (set a bunch of register on host)
24  [   52.720940] dw_mipi_dsi_encoder_enable: dw_mipi_dsi_pre_enable

>> drm_panel_prepare(dsi->panel); 
25  [   52.723598] panel_simple_prepare: set enable_gpio to 1
26  [   52.743886] panel_simple_prepare: set reset_gpio to 1
27  [   52.765115] panel_simple_prepare: set reset_gpio to 0

>> >>  send command via the host dsc interface (previously powered on already)
>> >>  the 0x34 register should be in command mode
28  [   52.786408] panel_simple_prepare: send on_cmds
29  [   53.052037] panel_simple_prepare: send on_cmds done err 0

>> In this function, the DSI_PWR_UP is reset and power up again, why?
30  [   53.052071] dw_mipi_dsi_encoder_enable: dw_mipi_dsi_enable
31  [   53.052100] dw_mipi_dsi_enable: debuga start

>> turn on the backlight
32  [   53.052149] panel_simple_enable: debuga start
33  [   53.329619] panel_simple_enable: debuga backlight_enable ffffffc07ae64400
```

Now the question is that why the DSI module is reset and power up again? 

During reset and power up period, if vid mode is none-zero, it will be set:

Register `MIPIC_VID_MODE_CFG` 0x00038 bit 1:0, vid_mode_type:

```
This field indicates the video mode
transmission type as follows: 
00:Non-burst with sync pulses 
01:Non-burst with sync events
10 and 11:Burst with sync pulses
```

MIPIC_MODE_CFG 0x00034

```
en_video_mode
When set to 1,this bit enables the DPI Video mode transmission.
```

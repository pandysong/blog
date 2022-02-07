---
date: 2022-02-06
title: How to Understand the Regster Setting in Rockchip RGA Driver's Alpha Mode
weight: 10
---

# Alpha Mode

Alpha mode allows two RGBA images to blend to one RGBA image. So there are two
parts of the algorithms.

## RGB Calculation

Following figure from datasheet defines how the destination RGB is calculated

![how rgb is calculated](/img/rga_alpha_mode_color_calculation.png)

Some notes:

- As0' is calculated from As0:

    - Either As0' is As0 (with sw_src_alpha_m0 = 0b0 )
    - Or As0' is 255- As0 (with sw_src_alpha_m0 = 0b1 )

- As0_" is calcuated from As0' and Ags

    - As0_" = Ags or
    - As0_" = As0' or
    - As0_" = (As0'\*Ags)>>8

- Fd0 has 5 possible values

    - 0
    - 256
    - As0"
    - 256-As0"
    - Ad0"

Finally the destination color is calculated with following formula:

Cd = Fs0 * Cs' + Fd0 * Cd'

In my case, I want to blend two images with the global src and dst alpha value.
Normally alpha = 0 means totally transparent while alpha == 255 means totally
opaque.

So I should have following configuration:

- Cs' = Cs , with following Regster setting:

    - sw_src_color_m0 = 0b0

- Cd' = Cd , with following Regster setting:

    - sw_dst_color_m0 = 0b0

- Fd0 = Ad0" = Ad0_" = Agd , with following Regster setting:

    - sw_dst_factor_m0 = 0b100
    - sw_dst_alpha_cal_m0 = 0b1
    - sw_dst_blend_m0 = 0b00
    - sw_dst_global_alpha with the requested alpha value

- Fs0 = As0" = As0_" = Ags , with following Regster setting:

    - sw_src_factor_m0 = 0b100
    - sw_src_alpha_cal_m0 = 0b1
    - sw_src_blend_m0 = 0b00
    - sw_src_global_alpha with the requested alpha value


## Alpha Calculation

![how rgb is calculated](/img/rga_alpha_mode_alpha_calculation.png)

The final alpha value is calculated from following formula:

Ad = Fs1 * As1" + Fd1 * Ad1"

In my case I need to force the final alpha to be 255, so

- Fd1 = 255 (The 256 value in the figure should be an typo)
- Fs1 = 255

With following register settings:

- sw_dst_factor_m1 = 0b001
- sw_src_factor_m1 = 0b001


# Mapping to User Space Message rga2_msg


-    sw_dst_global_alpha     : msg->src_a_global_val
-    sw_src_global_alpha     : msg->dst_a_global_val

-    sw_dst_color_m0 (1b)    : msg->alpha_mode_0 >> 15
-    sw_src_color_m0 (1b)    : msg->alpha_mode_0 >> 7
-    sw_dst_factor_m0(3b)    : msg->alpha_mode_0 >> 12
-    sw_src_factor_m0(3b)    : msg->alpha_mode_0 >> 4
-    sw_dst_alpha_cal_m0(1b) : msg->alpha_mode_0 >> 11
-    sw_src_alpha_cal_m0(1b) : msg->alpha_mode_0 >> 3
-    sw_dst_blend_m0(2b)     : msg->alpha_mode_0 >> 9
-    sw_src_blend_m0(2b)     : msg->alpha_mode_0 >> 1

Following python code get the requested alpha_mode_0 value:

```python
sw_src_color_m0 = 0b0
sw_dst_color_m0 = 0b0
sw_dst_factor_m0 = 0b100
sw_dst_alpha_cal_m0 = 0b1
sw_dst_blend_m0 = 0b00
sw_src_factor_m0 = 0b100
sw_src_alpha_cal_m0 = 0b1
sw_src_blend_m0 = 0b00

alpha_mode_0 = 0
alpha_mode_0 |= sw_dst_color_m0    << 15
alpha_mode_0 |= sw_src_color_m0    << 7
alpha_mode_0 |= sw_dst_factor_m0   << 12
alpha_mode_0 |= sw_src_factor_m0   << 4
alpha_mode_0 |= sw_dst_alpha_cal_m0<< 11
alpha_mode_0 |= sw_src_alpha_cal_m0<< 3
alpha_mode_0 |= sw_dst_blend_m0    << 9
alpha_mode_0 |= sw_src_blend_m0    << 1

print("0x{:x}".format(alpha_mode_0))

# here we get 0x4848

```

Following python code get the requested alpha_mode_0 value:

```
sw_dst_factor_m1 = 0b001
sw_src_factor_m1 = 0b001
alpha_mode_1 = 0
alpha_mode_1 |= sw_dst_factor_m1 << 12
alpha_mode_1 |= sw_src_factor_m1 << 4
print("0x{:x}".format(alpha_mode_1))

# here we get 0x1010
```

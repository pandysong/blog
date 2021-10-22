---
date: 2020-11-06
title: Sound Card Driver
weight: 10
---

# Introduction


# Concepts

## DAI

DAI is a Digital Audio Interface. So a I2S on the Soc is a DAI and a Codec is a
DAI.  There are drivers for I2S. While there are seperate driver for those
codecs. So how those DAIs are bound together to form a complete sound card
device for upper layer?

It is very similar with MIPI driver where thers is a host driver and panel
driver. The I2S driver is in the same postion of Host Driver while Codec driver
is of panel driver.

# How these two drivers are bound together

The "simple-audio-card" driver will associate the two DAI together:

```
 rk809_sound: rk809-sound {
     compatible = "simple-audio-card";
     simple-audio-card,format = "i2s";
     simple-audio-card,name = "rockchip,rk809-codec";
     simple-audio-card,mclk-fs = <256>;
     simple-audio-card,widgets =
         "Microphone", "Mic Jack",
         "Headphone", "Headphone Jack";
     simple-audio-card,routing =
         "Mic Jack", "MICBIAS1",
         "IN1P", "Mic Jack",
         "Headphone Jack", "HPOL",
         "Headphone Jack", "HPOR";
     simple-audio-card,cpu {
         sound-dai = <&i2s0_8ch>;
     };
     simple-audio-card,codec {
         sound-dai = <&rk809_codec>;
     };
 };
```

So this sound card contains (logically) two DAI:

the cpu dai "i2s0_8ch" and "rk809_codec" the external codec.


During sound card startup, the cpu dai's clock is firstly enabled.


```
static int asoc_simple_card_startup(struct snd_pcm_substream *substream)
{
    struct snd_soc_pcm_runtime *rtd = substream->private_data;
    struct simple_card_data *priv = snd_soc_card_get_drvdata(rtd->card);
    struct simple_dai_props *dai_props =
        simple_priv_to_props(priv, rtd->num);
    int ret;

    ret = asoc_simple_card_clk_enable(&dai_props->cpu_dai);
    if (ret)
        return ret;

    ret = asoc_simple_card_clk_enable(&dai_props->codec_dai);
    if (ret)
        asoc_simple_card_clk_disable(&dai_props->cpu_dai);

    return ret;
}
```

The cpu dai's clock is set via DTS.  clocks and clock-names are used.


```
 i2s0_8ch: i2s@ff800000 {
     compatible = "rockchip,rv1126-i2s-tdm";
...
     clocks = <&cru MCLK_I2S0_TX>, <&cru MCLK_I2S0_RX>, <&cru HCLK_I2S0>;
     clock-names = "mclk_tx", "mclk_rx", "hclk";
...
 };
```


```
rockchip_i2s_hw_params
```

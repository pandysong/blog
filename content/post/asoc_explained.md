---
date: 2021-09-19
title: ALSA Soc (ASoC) Driver Explained
weight: 10
---

# Terminology 

- DAI (Digital Audio Interface)

  This interface is the one between SoC and Codec. This has to be abstracted so
that the concept could be used both on SoC and Codec driver and could be
matched if they are connected on the same I2S interface. How is the matching
done in the high level? In short, it is done in Machine Driver. We will
explained it in details.

- Widget

  Each codec might be different. For example, a I2S PA, which receives I2S data
from I2S bus and playback to a speaker. This PA (represented as a Codec) could
have multiple internal knob where e.g. Volume could be adjusted or where it
could be digitally muted. 

  Another important reason to have widget is to control the power to minimal
needed level, using DAPM. A list of widget could be defined using helper macro
e.g. SND_SOC_DAPM_AIF_IN to define a widget which is a Audio Interface as Input
to the codec.

- Route

  There might be multiple widgets in the same codec. And audio could have
multiple different paths in the same codec. Or audio could have multiple
possible paths in the machines. `Route` add some constrains on how the audio
could be flowed in between widgets.

# Three Important Logical Parts

## Codec Driver

 A codec driver represents a type of codec which is independent from a SoC,
which means it is portable from one SoC to another and could be reused.

## SoC Driver (Platform Driver)

A SoC driver is coupled with a SoC. So this part could not be ported from one
platform to another.

## Machine Driver (gluing Codecs and SoC)

A Machines driver glues codecs and the soc to form a complete Sound Card to
upper layer. A generic implementation of a machine driver is simple-audio-card.

How the gluing is actually done?

Refer to:

```
devicetree/bindings/sound/simple-card.txt
```

For example, this device tree of a simple-audio-card, glue the following two
parts

- the CPU part (SoC driver): rcar_sound
- The Codec Part (Codec Driver): ak4643

```
    sndcpu: simple-audio-card,cpu {
        sound-dai = <&rcar_sound>;
    };

    sndcodec: simple-audio-card,codec {
        sound-dai = <&ak4643>;
        system-clock-frequency = <11289600>;
    };
```

# High Level Process

For ALSA, the high level interface is the sound card. When High Level Software
instructs to play, it will need to find the correct path from DAI to the
speaker, and turn on the widget on that path so that audio data could flow
through the path to speaker.


# Fixed Name of Widget

In following core code:

```
kernel/sound/soc/soc-core.c
```

Following fixed name widget is defined as well-known widget

```
static const struct snd_soc_dapm_widget simple_widgets[] = {
      SND_SOC_DAPM_MIC("Microphone", NULL),
      SND_SOC_DAPM_LINE("Line", NULL),
      SND_SOC_DAPM_HP("Headphone", NULL),
      SND_SOC_DAPM_SPK("Speaker", NULL),
  };
```



```
struct snd_soc_dapm_widget {
      enum snd_soc_dapm_type id;
      const char *name;       /* widget name */
      const char *sname;  /* stream name */
      struct list_head list;
      struct snd_soc_dapm_context *dapm;

      void *priv;             /* widget specific data */
      struct regulator *regulator;        /* attached regulator */
      struct pinctrl *pinctrl;        /* attached pinctrl */
      const struct snd_soc_pcm_stream *params; /* params for dai links */
      unsigned int num_params; /* number of params for dai links */
      unsigned int params_select; /* currently selected param for dai link */
```
## widget.txt Document

The document in widget.txt explain it as well. The "template-wname" is the
template widget name. This allows to define a user-supplied-name to the widget.

```
Widgets:

This mainly specifies audio off-codec DAPM widgets.

Each entry is a pair of strings in DT:

    "template-wname", "user-supplied-wname"

The "template-wname" being the template widget name and currently includes:
"Microphone", "Line", "Headphone" and "Speaker".

The "user-supplied-wname" being the user specified widget name.

For instance:
    simple-audio-widgets =
        "Microphone", "Microphone Jack",
        "Line", "Line In Jack",
        "Line", "Line Out Jack",
        "Headphone", "Headphone Jack",
        "Speaker", "Speaker External";
```

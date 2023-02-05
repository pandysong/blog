---
date: 2022-09-28
title: Wechat Audio Decode
weight: 10
---


# strip the first byte

```
dd if=<inputfile.silk> of="outputfile.silk" bs=1 skip=1
```

#  Download Silk Codec

```
https://github.com/gaozehua/SILKCodec
```

compile it and using decoder to decode it

```
usage: ./decoder in.bit out.pcm [settings]

in.bit       : Bitstream input to decoder
out.pcm      : Speech output from decoder
   settings:
-Fs_API <Hz> : Sampling rate of output signal in Hz; default: 24000
-loss <perc> : Simulated packet loss percentage (0-100); default: 0
-quiet       : Print out just some basic values
```

# using audacity to import this raw data


The default sample rate is 24K as shown above. 

---
date: 2020-04-23
title: low cost IPC tear down
weight: 10
---

# Low cost IPC

It happened that we have an broken IPC:

![landscape](/img/ipc/landscape-01.jpg)
![landscape](/img/ipc/landscape-02.jpg)


# main board

## Main components and connectors on the front side:

![mainboard-01](/img/ipc/mainboard-01.jpg)

1. the main chip, ANYKA
   [AK3918EN080][https://www.anyka.com/en/productInfo.aspx?id=105], which is a
highly integrated SoC with ARM9 processor, camera interface with ISP and with
integrated DDR2 and H.264 encoder.
2. AJ1816, the 8MB Flash
3. The common ground to LED light source board.
4. Lens Mounting Screw 1
5. Lens Mounting Screw 2
6. Connected to Lens (Auto Focus? not likely)
7. Microphone connector (Microphone is installed on a hole on the case)
8. RS232 Transceiver?


The program is loaded from SPI flash and runs in internal DDR. RTSP streaming
is done in software while the video encoding is accelerated by H.264 hardware
encoder.

## Main components on the back side

![mainboard-02](/img/ipc/mainboard-02.jpg)

1. RTL8201F, 100M Ethernet Phy
2. CHMC S62, Amplifier
3. Image Sensor

## Lens and Base

![lens and lens base](/img/ipc/lens-and-base-01.jpg)
![lens and lens base](/img/ipc/lens-and-base-02.jpg)


# Power Supply Board

![power-supply-01](/img/ipc/power-supply-01.jpg)


1. Ethernet Transformer for TX and RX
2. Transformer?
3. Broken Capacitor
4. Common Ground Connector to the case
5. External: DC Connector (12V, GND, 78?, 45?)
6. External: Ethernet T+,T-, R+, R-; and LED1/2. 
7. The connector to mainboard: 
   GND, 12V, T+, T-, R+, R-, LED1, LED2

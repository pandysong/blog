---
date: 2019-04-26
title: high speed signals on fpga
weight: 10
---

# HDMI vs MIPI

Both seem the high speed signal, however it looks HDMI is more complex than
MIPI CSI/DSI.

HDMI have some competing IP provider. While for MIPI, there are some open
source project which could encode and decode the MIPI signals. Without digging
into the details, the reason behind that is the HDMI is machine to machine
interface which could be more comprehensive while for MIPI, it is inter-chip
interface, which is used to transfer the data reliably, without consider much
from end user point of view.


# MIPI CSI Open source project

A nice project:

https://github.com/circuitvalley/mipi_csi_receiver_FPGA

Where the MIPI CSI-2 receiver implemented is provided.


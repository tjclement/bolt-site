---
title: Logic Analyzer
excerpt: Using the Curious Bolt as a logic analyzer in PulseView
layout: docs
weight: 10
---

Your Bolt has an 8-channel logic analyzer with 100k pts sample memory. It's compatible with the popular open source PulseView software, and supports setting low/high level triggers.

![A screenshot of PulseView being used with Curious Bolt](../../../images/pulseview.png)

## Connecting in PulseView
- Install [Pulseview](https://sigrok.org/wiki/Downloads)
- Connect your Curious Bolt via USB
- Open PulseView, and click on "No Device" dropdown
![](../../images/pulseview_connect1.png)
- Select the "SUMP Compatibles" driver, select the first USB serial port of the two exposed by Curious Bolt, set the baudrate to 115200, click on Scan, and then Ok
![](../../images/pulseview_connect2.png)
- See [Pulseview documentation](https://www.sigrok.org/doc/pulseview/0.4.1/manual.html) for how-to of signal capture and protocol decoding

<div class="info">
<strong>Note:</strong>
Triggering via the Python API does not cause the logic analyzer to trigger an update in PulseView.<br><br>

To trigger PulseView, you need to set a high or low level trigger on one of <a href="../pinout">the 8 input pins</a> in the PulseView UI.
</div>
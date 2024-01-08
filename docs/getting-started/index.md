---
title: Getting Started
excerpt: Getting started with Curious Bolt
layout: docs
---

## Setting up your Bolt
The Curious Bolt exposes a serial port over USB that can be used to interface with its glitcher, scope, and programmable I/O.

A python library is available to talk to your Bolt. To use it:

- Install _python_ (3.x) and _pip_
- Install _pyserial_ by running `pip install pyserial`
- Copy [scope.py](https://github.com/tjclement/bolt/blob/main/lib/scope.py) to your working directory
- Check if you can connect by running `python3 -c "from scope import Scope;s=Scope()"`. If you get a warning that the Bolt could not be found, check its USB connection.
- If you run Linux and can't connect, check [your udev rules](linux_udev).

## Logic analysis in PulseView
![A screenshot of PulseView being used with Curious Bolt](../../images/pulseview.png)
See [Logic Analyzer](logic_analyzer) to learn how to use the logic analyzer.

## Fault injection with the voltage glitcher
![](../../images/scope.jpg)
See [Voltage Glitching](voltage_glitching) for more information on voltage glitching with the Bolt.

## Programmable I/O
You can get the Bolt to send programmable output patterns on its output GPIO, based on triggers. See [Programmable I/O](programmable_io).

## Differential power analysis with the oscilloscope
![](../../images/scope_data.png)
See [Differential Power](scope) to learn how to use the differential power scope.

<div class="info">
<strong>Note:</strong>
The scope of your Bolt is not necessary for any of the Level 1 challenges.
</div>

Learn more about your Bolt in these sections:
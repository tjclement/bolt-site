---
title: Differential Power
excerpt: Differential Power Analysis with the scope
layout: docs
weight: 40
---

Your Curious Bolt has a 35MSPS differential scope that can measure voltage changes of your target's power rails. It measures the AC component (changes in the DC voltage) of the power rails, and detects from -0.2V below your target's nominal voltage to +0.2V above it in steps of 0.2mV. It has 50k measurements sample memory.

![](/images/scope_data.png)

## Connecting to your target

Connect one (ideally both) of the GND pins next to [the "ADC" marking on the Bolt](../pinout) to your target board's ground. Then connect the Bolts ADC pin to the positive power of your target, as close to the target chip as you can ("VMCU" on the Challenge Level 1 board).

## Power scope API

Interfacing with the differential power scope functionality is done directly with the `Scope` object in the Python library.

```python
import time
from scope import Scope
s = Scope()

# Trigger once to collect samples
s.trigger()

# Then, either print the collected samples
s.get_last_trace()

# or draw a graph from them
s.plot_last_trace()
```
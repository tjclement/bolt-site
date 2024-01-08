---
title: Level 1 Challenges
excerpt: Target - Level 1
layout: docs
---

![](/images/target_back.png)

The first level target is a board containing an STM32F103, that has all security options available to it enabled. JTAG/SWD is turned off, flash readout is disabled, and the firmware it's shipped with contains no (known) software vulnerabilities.

The board has 4 challenges, each started by holding a button on the board. It's up to you to break the target's security and obtain the flag from each challenge.

Flags are in the format `ctf{...}`, with 34 uppercase alphanumeric characters where the `...` are. The flags are shown in plaintext when you obtain them, and always contain the `ctf{}` part too, so you never need to add that yourself.

<div class="info">
<h3>Note:</h3>
The target's USB port exposes a single USB UART interface. You can connect to it at 115200 baud using your favourite terminal to see what it's outputting.
</div>

![](/images/target_front.png)

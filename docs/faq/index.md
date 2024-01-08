---
title: FAQ
excerpt: FAQ
layout: docs
---

## Help, I bricked my target board!

If you've been playing with RDP, chances are you've softbricked your Level 1 target board. Not to worry, you can use [STM32CubeProgrammer](https://www.st.com/en/development-tools/stm32cubeprog.html) along with your STM32 SWD debugger to reflash the [challenge firmware](https://github.com/tjclement/ecsc23-badge/blob/main/production_flashing/ECSC23.bin) on your board. Be sure to enable RDP again in the Option Bytes ("OB") panel, otherwise the challenges are no fun. Reversing the linked binary is cheating, btw. Don't cheat. Or do 🏴‍☠️.

## Can I power a target board using Bolt output pins?

The output pins are protected by current-limiting resistors, to help keep your Bolt from releasing its magic smoke when the pins get accidentally shorted. This makes it hard to power devices from them directly.

Your safest option is to use the output pins to switch a MOSFET or relay that powers your target. If you really want to, and know for sure that the RP2040's max GPIO source current allows for it, you can also swap the 4 current-limiting resistors next to the output pins with 0-ohm ones. This allows maximum current through the output pins. Please be careful to not fry your Bolt!

***

## How can I get in touch with you?

We'd love to hear from you! Check out [this page](/docs/community) to learn how to reach us.

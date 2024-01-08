---
title: Challenge 1
excerpt: Challenge 1
layout: docs
weight: 10
---

In this challenge, the flag gets loaded into RAM in plaintext. There should be no way to extract it, as JTAG and SWD have been disabled.

You need to obtain it anyway.


<div class="note">
<h3>Hint 1</h3>
<button id="show-btn1" onclick="document.getElementById('show-btn1').style.display='none';document.getElementById('hint1').style.display='block';">Show</button>
<p id="hint1" style="display:none">
You need to find a way around the STM32F103's way of disabling JTAG/SWD.
</p>
</div>

<div class="note">
<h3>Hint 2</h3>
<button id="show-btn2" onclick="document.getElementById('show-btn2').style.display='none';document.getElementById('hint2').style.display='block';">Show</button>
<p id="hint2" style="display:none">
The STM32F103 has no e-fuses to configure JTAG/SWD disabling across reboot.
</p>
</div>

<div class="note">
<h3>Hint 3</h3>
<button id="show-btn3" onclick="document.getElementById('show-btn3').style.display='none';document.getElementById('hint3').style.display='block';">Show</button>
<p id="hint3" style="display:none">
The STM32F103's bootrom cannot disable JTAG/SWD.
</p>
</div>

<div class="important">
<h3>Solution</h3>
<button id="show-btn-sol" onclick="document.getElementById('show-btn-sol').style.display='none';document.getElementById('solution-content').style.display='block';">Show</button>
<span id="solution-content" style="display:none">
<p>In this challenge, the flag gets loaded into RAM in plaintext. There should be no way to extract it, as JTAG and SWD have been disabled.</p>

<p>STM32F1 series have a design vulnerability though, where the debugging peripheral can only be disabled in software, and not through hardware fuse bits. This means that it is still accessible when an attacker can force booting from bootrom or SRAM.</p>

<p>Because RAM is not cleared upon reset, the flag is still there, and it can be obtained by dumping RAM.</p>

<p>
<ul>
<li>Start the challenge by holding the chall1 button, and notice that the flag is now in SRAM.</li>
<li>Force booting into the bootrom bootloader by holding BOOT0 and pressing RST. JTAG/SWD is now accessible again.</li>
<li>Attach the ST-Link debugger to SWDIO/GND/SWCLK on the target board, and connect to your computer via USB.</li>
<li>Install `pyocd` using `pip3 install pyocd`.</li>
<li>Run pyocd in the command mode using `pyocd commander` </li>
<img src="../images/2_1.png">
<li>Dump the entire SRAM</li>
<img src="../images/2_2.png">
<li>Search for the flag preamble, and see that the flag is right there</li>
<img src="../images/2_3.png">
</ul>
</p>
</span>
</div>
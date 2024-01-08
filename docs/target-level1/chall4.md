---
title: Challenge 4
excerpt: Challenge 4
layout: docs
weight: 40
---

In this challenge, the flag is printed by a function that never gets called. Readout protection (RDP) is active, meaning the flash content can't be dumped, even from JTAG/SWD, the bootloader, or when booted from SRAM.



<div class="note">
<h3>Hint 1</h3>
<button id="show-btn1" onclick="document.getElementById('show-btn1').style.display='none';document.getElementById('hint1').style.display='block';">Show</button>
<p id="hint1" style="display:none">
The RDP setting is saved to non-volatile memory, unlike the earlier JTAG/SWD disable.
</p>
</div>

<div class="note">
<h3>Hint 2</h3>
<button id="show-btn2" onclick="document.getElementById('show-btn2').style.display='none';document.getElementById('hint2').style.display='block';">Show</button>
<p id="hint2" style="display:none">
RDP is honored even in bootrom, and when booted from RAM.
</p>
</div>

<div class="note">
<h3>Hint 3</h3>
<button id="show-btn3" onclick="document.getElementById('show-btn3').style.display='none';document.getElementById('hint3').style.display='block';">Show</button>
<p id="hint3" style="display:none">
RDP on STM32F103 specifically has been proven to be broken semi-recently.
</p>
</div>

<div class="note">
<h3>Hint 4</h3>
<button id="show-btn4" onclick="document.getElementById('show-btn4').style.display='none';document.getElementById('hint4').style.display='block';">Show</button>
<p id="hint4" style="display:none">
The solution involves voltage glitching, along with another approach.
</p>
</div>

<div class="important">
<h3>Solution</h3>
<button id="show-btn-sol" onclick="document.getElementById('show-btn-sol').style.display='none';document.getElementById('solution-content').style.display='block';">Show</button>
<span id="solution-content" style="display:none">
<p>You need to use a <a href="https://arxiv.org/pdf/2008.09710.pdf">recently published attack</a> that uses a vulnerability in the <a href="https://developer.arm.com/documentation/ddi0337/h/debug/about-the-flash-patch-and-breakpoint-unit--fpb-">ARM Flash Patch and Breakpoint peripheral</a> to enable flash access from SRAM-booted code, and then jump into the flag printing function.</p>

<p><ul>
<li>Checkout the vuln author's <a href="https://github.com/JohannesObermaier/f103-analysis/tree/master/h3">PoC code</a>.</li>
<li>This attack uses the FPB to patch the flash address space to SRAM, followed by a glitch-induced power cycle that resets the flash protection bit, without clearing SRAM. We are then booted from our own SRAM code, whilst the hardware thinks it's running from flash (thus enabling flash access).</li>
<li>Following the PoC's instructions, use the SWD-enabling trick from chall2 to upload the PoC shellcode into RAM using pyocd. Then do a single power glitch to reset, whilst holding BOOT0 and BOOT1 to boot from SRAM. <img src="../images/4_1.png"></li>
<li>Set up your serial terminal to 9600 baud (as that's the configured baud of the PoC code) <img src="../images/4_2.png"></li>
<li>If you reset the board using the RST button whilst holding down both BOOT pins, you'll see a simple prompt: <img src="../images/4_3.png"></li>
<li>Press RST once without the boot pins to enter stage 2 of the PoC shellcode.</li>
<li>If the glitched power cycle was successful, you can now dump the freshly-unprotected flash contents using the `d` command: <img src="../images/4_4.png"></li>
<li>Save the contents into a binary file, and open it in e.g. Ghidra (ARM Cortex, 32 bit, little-endian, load adddress 0x08000000).</li>
<li>(NB: at this point you can reverse, patch, and flash the firmware to the target board. Because STM32F103 does not have permanent RDP/write protection, the device manufacturer can't prevent you from flashing your own firmware. This approach is allowed. The intended flow is described below.)</li>
<li>Search for all strings, finding 'chall4 flag print function starts here', and go to the function that references it. You'll find the function at `0x08000BA8`, at the time of writing. Be sure to set bit 0 of the found address to 1 to have it executed in Thumb mode (so `0x08000BA9`).</li>
<li>Modify the PoC to add a new command that calls this function: 

<img src="../images/4_5.png"> <img src="../images/4_6.png"></li>
<li>Compile and upload the new shellcode using `pyocd` again, and execute your new command.</li>
<li>Note that the flag will be printed at `115200` baud, whilst your terminal is set to `9600`. You can e.g. use the logic analyzer to view the flag contents, or write a script that quickly changes the terminal's baud after sending the new command. <img src="../images/4_7.jpeg"></li>
</ul></p>
</span>
</div>

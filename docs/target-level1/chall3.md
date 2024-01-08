---
title: Challenge 3
excerpt: Challenge 3
layout: docs
weight: 30
---

In this challenge, the flag gets printed only when certain bits in external EEPROM memory are cleared (equal to 0).

Simply changing the bits on the EEPROM chip does not work, as an integrity check ensures that data on the chip has not been modified.

<div class="note">
<h3>Hint 1</h3>
<button id="show-btn1" onclick="document.getElementById('show-btn1').style.display='none';document.getElementById('hint1').style.display='block';">Show</button>
<p id="hint1" style="display:none">
The EEPROM communicates with the STM32 via i2c.
</p>
</div>

<div class="note">
<h3>Hint 2</h3>
<button id="show-btn2" onclick="document.getElementById('show-btn2').style.display='none';document.getElementById('hint2').style.display='block';">Show</button>
<p id="hint2" style="display:none">
You can use the logic analyzer to analyze this communication.
</p>
</div>

<div class="note">
<h3>Hint 3</h3>
<button id="show-btn3" onclick="document.getElementById('show-btn3').style.display='none';document.getElementById('hint3').style.display='block';">Show</button>
<p id="hint3" style="display:none">
You can change 1's on the i2c bus to 0's by glitching it to ground (or using programmable i/o).
</p>
</div>

<div class="note">
<h3>Hint 4</h3>
<button id="show-btn4" onclick="document.getElementById('show-btn4').style.display='none';document.getElementById('hint4').style.display='block';">Show</button>
<p id="hint4" style="display:none">
Does the integrity check happen everytime the data is read from EEPROM?
</p>
</div>

<div class="important">
<h3>Solution</h3>
<button id="show-btn-sol" onclick="document.getElementById('show-btn-sol').style.display='none';document.getElementById('solution-content').style.display='block';">Show</button>
<span id="solution-content" style="display:none">
<p>You can use the voltage glitcher as a means to corrupt data on the i2c bus, live in-transit.</p>

<p><ul>
<li>Connect to the glitcher in PulseView. <img src="../images/pulseview.png"></li>
<li>Start chall3 and observe that the flag is not printed due to protection bits (with value 0x0D). <img src="../images/3_1.png"></li>
<li>Hook up the scope's first two channels to the SCL and SDA pins of the target board, configure the SDA as a low edge trigger in Pulseview, and snoop the i2c bus traffic. You'll see two reads: one with data and a checksum, and then another with only data. <img src="../images/3_2.png"></li>
<li>Corrupting the first read will always trigger an error stating that the EEPROM data is invalid, yet the second read has no such errors.</li>
<li>Zooming in on the second read, we find that the protection bits (0x0D) are the 2nd byte that's being read. <img src="../images/3_3.png"></li>
<li>Hook up both the glitcher source and the third input channel to some pin on the target board that is default-high (e.g. RX), configure the glitcher to trigger based on a falling edge of SDA, and provide some educated guess for the glitch offset (how long after triggering to start the glitch) and the glitch duration. Start chall3 to have the glitcher trigger. <img src="../images/3_4.png"></li>
<li>You can study the duration and offset of your glitch in Pulseview, in the third channel: <img src="../images/3_5.png"></li>
<li>The trick is now to have your glitch completely overlap with the 0x0D byte value, such that these bits all get pulled down to 0x00: <img src="../images/3_6.png"></li>
<li>Once you've verified your glitch offset and duration is right, connect the glitch source pin to SDA, and arm the glitcher one last time to get the flag: <img src="../images/3_7.png"></li>
</ul></p>
</span>
</div>
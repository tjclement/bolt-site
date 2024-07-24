---
title: Challenge 2
excerpt: Challenge 2
layout: docs
weight: 20
---

In this chall, the flag is protected by a check that is never false:

```c
  volatile bool check = true;
  uint32_t cnt = 0;
  int i = 0;
  int j;

  while (true) {
    cnt = 0;
    for (i = 0; i < 1000; i++) {
      for (j = 0; j < 1000; j++) {
        cnt++;
        if (!check) {
          // Flag gets printed
        }
      }
    }
    uart_printf("%u %u %u\r\n", i, j, cnt);
  }
```


<div class="note">
<h3>Hint 1</h3>
<button id="show-btn1" onclick="document.getElementById('show-btn1').style.display='none';document.getElementById('hint1').style.display='block';">Show</button>
<p id="hint1" style="display:none">
There is no software vulnerability, as clearly the check can never be true.
</p>
</div>

<div class="note">
<h3>Hint 2</h3>
<button id="show-btn2" onclick="document.getElementById('show-btn2').style.display='none';document.getElementById('hint2').style.display='block';">Show</button>
<p id="hint2" style="display:none">
Voltage glitching can inject faults such as skipping instructions or corrupting computations.
</p>
</div>

<div class="note">
<h3>Hint 3</h3>
<button id="show-btn3" onclick="document.getElementById('show-btn3').style.display='none';document.getElementById('hint3').style.display='block';">Show</button>
<p id="hint3" style="display:none">
A good way to find the right glitch duration to corrupt your target is to start with a very long duration that resets (crashes) the target, and then decrease it until right below where resets occur. This is where maximum disruption without crashing happens.
</p>
</div>

<div class="note">
<h3>Hint 4</h3>
<button id="show-btn4" onclick="document.getElementById('show-btn4').style.display='none';document.getElementById('hint4').style.display='block';">Show</button>
<p id="hint4" style="display:none">
If you see a reset on a certain glitch duration, and just decreasing the glitch duration by 1 doesn't reset anymore but also doesn't seem to corrupt the counters printed to the screen at all, try different glitch cables between the Bolt and target board. The DuPont cables of the ST-Link that came with your Bolt should work. Connect 2x GND and 1x SIG from the Bolt to VMCU on the target, using the 3 pin header on the target. If you're still having trouble with this challenge, please reach out on <a href="../community">our discord server</a> so we can help out.
</p>
</div>

<div class="important">
<h3>Solution</h3>
<button id="show-btn-sol" onclick="document.getElementById('show-btn-sol').style.display='none';document.getElementById('solution-content').style.display='block';">Show</button>
<span id="solution-content" style="display:none">
<p>By glitching the target board's VCC with the correct duration, register values can be corrupted and instructions can be skipped, causing the conditional check to get corrupted and the flag to be printed.</p>

<p>
<ul>
<li>Use the DuPont cables that came with the ST-Link in your Bolt kit.</li>
<li>Connect the two GND pins of the glitcher output to the two GND pins on the target's GND/VMCU/GND pin header (using two GND cables instead of one helps make the glitch signal more crisp).</li>
<li>Connect the glitcher's glitch output pin (SIG) to the targets VMCU pin header (you can also use a probe on the VMCU probe testpoint on the board, but it's harder to get a reliable glitch that way).</li>
<li>Start the challenge by holding the chall2 button.</li>
<li>Find the smallest glitch duration that triggers a complete reset of the target board, using e.g. `python3 -c "from scope import Scope;s=Scope();s.glitch.repeat=92;a=[s.trigger() for i in range(50000)]"` (s.glitch.repeat is the number of 8.3ns clock cycles to keep the glitch asserted). This continuously triggers a glitch, and you can press ctrl+C to stop it.</li>
<li>Slowly walk down the glitch duration (or, if you'd like, perform a binary search by hand for faster results) such that the target does not reset, but `i`, `j`, and `cnt` do get corrupted. Running the challenge long enough with these corruptions results in getting the flag:</li>
<img src="../images/1.png">
</ul>
</p>

<p>
Note: if corruption does occur, but the flag doesn't get printed, try stopping the glitch, power cycling the target board, run CHALL2, and start glitching again.
</p>
</span>
</div>


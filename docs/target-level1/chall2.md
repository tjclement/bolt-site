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

<div class="important">
<h3>Solution</h3>
<button id="show-btn-sol" onclick="document.getElementById('show-btn-sol').style.display='none';document.getElementById('solution-content').style.display='block';">Show</button>
<span id="solution-content" style="display:none">
<p>By glitching the target board's VCC with the correct duration, register values can be corrupted and instructions can be skipped, causing the conditional check to get corrupted and the flag to be printed.</p>

<p>
<ul>
<li>Connect the glitcher's GND (next to the glitch source) to GND on the target (e.g. the middle pin of the 3-pin SWD header).</li>
<li>Connect the glitcher's glitch source pin to the targets VCC header (either of the two VCC pins on the target are fine).</li>
<li>Find the smallest glitch duration that triggers a complete reset of the target board, using e.g. `python3 -c "from scope import Scope;s=Scope();s.glitch.repeat=92;s.trigger()"` (s.glitch.repeat is the number of 8.3ns clock cycles to keep the glitch asserted).</li>
<li>Start the challenge by holding the chall2 button, and slowly walk down the glitch duration such that the target does not reset, but `i`, `j`, and `cnt` do get corrupted. Running the challenge long enough with these corruptions results in getting the flag:</li>
<img src="../images/1.png">
</ul>
</p>

<p>
Note: if corruption does occur, but the flag doesn't get printed, try stopping the glitch, power cycling the target board, run chall1, and start glitching again.
</p>
</span>
</div>


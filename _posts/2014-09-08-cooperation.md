---
layout: post
title: "Cooperation on shared Linux servers"
---

<p>If you're about to run computationally intensive jobs on a public or
departmental server
(such as one of the impact servers at Iowa State), please help share
the resources. Here are some favors you can do.</p>

<h3>1. Kill old processes you don't need.</h3>

<p>I recommend "top". I can look up my own processes with</p>
<pre><code>$ top -u landau
</code></pre>

<p>and I get a table of processes.</p>

<pre><code>PID   USER   PR  NI  VIRT  RES  SHR  S  %CPU   %MEM TIME+    COMMAND
19522 landau 39  19  128g  65m  24m  R  100.4  0.1  85:46.54 R
20255 landau 20  0   26328 1732 1068 R  0.7    0.0  0:00.03  top
19516 landau 39  19  103m  1320 1128 S  0.0    0.0  0:00.00  sh
20015 landau 20  0   100m  2036 1076 S  0.0    0.0  0:00.09  sshd
20016 landau 20  0   117m  3324 1636 S  0.0    0.0  0:00.12  bash
</code></pre>

<p>From looking at the %CPU column, I see that my R process
 is the most expensive (see the COMMAND column) and has been running
 for a long time (TIME+). If I don't need this process anymore
 or if I need to restart my workflow, I should kill it. I</p>

<ol>
<li>Type "k" within top.</li>
<li>Type the PID (process ID) of the process to kill (19522 in this case).</li>
<li>Press enter.</li>
</ol>

<h3>2. Ballpark the number of free CPU cores. Then, spawn no more than one expensive process per core you plan to use.</h3>

<p>Within top, press "1" to get the following table.</p>

<pre><code>Cpu0  :  0.0%us,  0.3%sy,  0.0%ni, 99.7%id,  0.0%wa,  0.0%hi,  0.0%si, ...
Cpu1  :  0.0%us,  0.0%sy,  0.0%ni,100.0%id,  0.0%wa,  0.0%hi,  0.0%si, ...
Cpu2  :100.0%us,  0.0%sy,  0.0%ni,  0.0%id,  0.0%wa,  0.0%hi,  0.0%si, ...
Cpu3  :100.0%us,  0.0%sy,  0.0%ni,  0.0%id,  0.0%wa,  0.0%hi,  0.0%si, ...
Cpu4  :  0.0%us, 29.4%sy, 70.6%ni,  0.0%id,  0.0%wa,  0.0%hi,  0.0%si, ...
Cpu5  :100.0%us,  0.0%sy,  0.0%ni,  0.0%id,  0.0%wa,  0.0%hi,  0.0%si, ...
Cpu6  : 81.1%us,  0.0%sy,  0.0%ni, 18.9%id,  0.0%wa,  0.0%hi,  0.0%si, ...
Cpu7  : 17.3%us,  0.0%sy,  0.0%ni, 82.7%id,  0.0%wa,  0.0%hi,  0.0%si, ...
Cpu8  :  7.3%us,  0.0%sy,  0.0%ni, 92.7%id,  0.0%wa,  0.0%hi,  0.0%si, ...
Cpu9  : 65.4%us,  0.0%sy,  0.0%ni, 34.6%id,  0.0%wa,  0.0%hi,  0.0%si, ...
Cpu10 : 46.0%us,  0.3%sy,  0.0%ni, 53.6%id,  0.0%wa,  0.0%hi,  0.0%si, ...
Cpu11 : 82.5%us,  0.0%sy,  0.0%ni, 17.5%id,  0.0%wa,  0.0%hi,  0.0%si, ...
Cpu12 :100.0%us,  0.0%sy,  0.0%ni,  0.0%id,  0.0%wa,  0.0%hi,  0.0%si, ...
Cpu13 :100.0%us,  0.0%sy,  0.0%ni,  0.0%id,  0.0%wa,  0.0%hi,  0.0%si, ...
Cpu14 :  0.0%us,  0.0%sy,  0.0%ni,100.0%id,  0.0%wa,  0.0%hi,  0.0%si, ...
Cpu15 :  0.0%us,  0.0%sy,  0.0%ni,100.0%id,  0.0%wa,  0.0%hi,  0.0%si, ...
Cpu16 :  0.0%us,  0.0%sy,  0.0%ni,100.0%id,  0.0%wa,  0.0%hi,  0.0%si, ...
Cpu17 :  0.0%us,  0.0%sy,  0.0%ni,100.0%id,  0.0%wa,  0.0%hi,  0.0%si, ...
Cpu18 :100.0%us,  0.0%sy,  0.0%ni,  0.0%id,  0.0%wa,  0.0%hi,  0.0%si, ...
Cpu19 :100.0%us,  0.0%sy,  0.0%ni,  0.0%id,  0.0%wa,  0.0%hi,  0.0%si, ...
Cpu20 :100.0%us,  0.0%sy,  0.0%ni,  0.0%id,  0.0%wa,  0.0%hi,  0.0%si, ...
Cpu21 :100.0%us,  0.0%sy,  0.0%ni,  0.0%id,  0.0%wa,  0.0%hi,  0.0%si, ...
Cpu22 : 99.7%us,  0.3%sy,  0.0%ni,  0.0%id,  0.0%wa,  0.0%hi,  0.0%si, ...
Cpu23 :100.0%us,  0.0%sy,  0.0%ni,  0.0%id,  0.0%wa,  0.0%hi,  0.0%si, ...
</code></pre>

<p>There are 24 cores, and I can see how they spend their time.</p>

<pre><code>us: user cpu time (or) % CPU time spent in user space
sy: system cpu time (or) % CPU time spent in kernel space
ni: user nice cpu time (or) % CPU time spent on low priority processes
id: idle cpu time (or) % CPU time spent idle
wa: io wait cpu time (or) % CPU time spent in wait (on disk)
hi: hardware irq (or) % CPU time spent handling hardware interrupts
si: software irq (or) % CPU time spent handling software interrupts
st: steal time, % CPU time in involuntary wait by virtual cpu ...
</code></pre>

<p>Only 8 cores have more than 80%id (time spent idling),
so spawning more than 8 expensive concurrent processes may slow down
or even crash the server. Please leave some cores free for other people.</p>

<h3>3. Be nice.</h3>

<p>The Linux tool <code>nice</code> modifies the scheduling priority of your job so that
 more urgent jobs can work around it. Setting a high niceness level slows down
 your work in the presence of more urgent jobs, but it helps the server schedule
 its workflow and makes you a good citizen. Niceness levels range from 0 to 19,
 and the higher the level, the nicer you are. I can submit an R job with a
 niceness level of 19 with</p>

<pre><code>$ nohup nice -19 R CMD BATCH job.R &
</code></pre>

<p>(I included the ampersand at the end and nohup at the beginning to run the job
in the background and keep it running after I log out.) To verify my job's niceness,
I can run top and look at the NI column of the default table.</p>

<h3>Final remarks</h3>

<p>To get <code>nice</code> to work, you may have to change your default
shell to <code>bash</code>. If you're at Iowa State, you can follow
the instructions in the September 9, 2014 edit of <a href="http://wlandau.github.io/2013/08/13/Rversion">
this post</a>. Otherwise, contact your institution or follow a guide like
<a href="http://stackoverflow.com/questions/13046192/changing-default-shell-in-linux">
this one</a>.</p>

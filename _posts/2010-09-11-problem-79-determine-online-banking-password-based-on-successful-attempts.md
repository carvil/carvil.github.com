---
layout: post
title: "Determine online banking password based on successful attempts"
category: Programming
tags: ["project_euler"]
---
{% include JB/setup %}

I solved the [79th Euler problem](http://projecteuler.net/index.php?section=problems&id=79) with pen
and paper, since the input data was rather small (50 attempts, with
repetitions). The ideia is to find the shortest possible secret passcode based
on a log fie containing 50 successful login attempts. Furthermore, the
characters are asked in order, which eases the process of finding the
solution. So, I started with the first one: 319. This means that the number 3
will appear before 1 and 9; the number 1 will appear after 3 and before 9;
finally, the number 9 will appear after 3 and 1. Then I continued with all the
numbers in the log file. At the end, I just run a quick walkthrough the log
file again to be sure. I ended up with the right solution: the password is
73162890.


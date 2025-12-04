---
layout: post
title:  "Advent Of Code 2025 - Answer Visualisations"
date:   2025-12-04 11:15 +0000
categories: aoc
tags: aoc aoc2025 python png
---

_I'll just update the one post this year. If I do more visualisations that is_

# Day 4 - Printing Department

or: [**Fun With Forklifts and Paper Rolls**](https://adventofcode.com/2025/day/4)

Part 1 of this involves searching for a set of paper rolls in a map that can be "reached by a forklift". It's not really visualisation bait, not least of all because the answer you seek is really only the very first step of the Part 2 problem:

![Day 4 Part 1 Solution](/assets/day4part2_1.png)
{:style="display: block; margin: auto; width: 50%;"}

Part 2 is more interesting - iteratively remove the set of paper rolls until you can't do any more. This is of course catnip to the visualisers, so obviously I had to have a go. Here's an animation of the sequence of removal. My solution took 75 steps to complete the process; it starts off impressively with a large number of updates but as we get closer to the end it can be hard to visually track the small number of red dots indicating next-to-remove:
<hr>
<center>
  <video width="560" height="560" controls="controls">
    <source src="/assets/aoc2025_day4part2.webm" type="video/webm; codecs=vp9">
  </video>
</center>
<hr>
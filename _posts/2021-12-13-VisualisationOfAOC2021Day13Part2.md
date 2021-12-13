---
layout: post
title:  "Visualisation of AOC 2021 Day 13, Part 2 answer"
date:   2021-12-13 12:30 +0000
categories: aoc2021
tags: aoc aoc2021 python png
---

Another day, another AOC challenge begging to be visualised. Smarter people
than me would animate the _actual_ folding operations nicely; I'm not so 
hot at that sort of thing. Instead what we've got is a scaled output (so as 
the grid shrinks in size after each fold the output stays the same size - the 
effect being that the dots get stretched) of each folding operation, ending 
up with the solution shown on the screen for all to see.

The code used to generate it (but, again, not the data[^1]) is here:
[python/day13part2_extracredit.py](https://github.com/henley-regatta/adventofcode2021/blob/main/python/day13part2_extracredit.py)

<video width="600" height="600" controls="controls">
    <source src="/assets/aoc2021_day13part2.webm" type="video/webm; codecs=vp9">
</video>


(If that doesn't work, Click [here](/assets/aoc2021_day13part2.webm) to view
the video directly) 

***

[^1]: Although today, that's totally ineffective security-by-obscurity because my actual answer is embedded in the output...

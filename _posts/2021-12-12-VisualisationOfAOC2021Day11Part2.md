---
layout: post
title:  "Visualisation of AOC 2021 Day 11, Part 2 answer"
date:   2021-12-12 18:20 +0000
categories: aoc2021
tags: aoc aoc2021 python png
---

As with previous attempts, this one was absolutely screaming to be visualised.
The challenge was "At what step do the Octopus Flashes become synchronised"; it
sounded like a combinatorial trap but it wasn't, it was a reasonable range and
once that happens it's begging to be turned into an animation. Here's a 
heat-map style animation with lighter colours being closer to a flash and the
big black screen at the end being the result of all the squares flashing at once.

What I find most interesting about this is how little structure is visible in
the pattern of flashes until nearly the very end when all of a sudden there's
clear movement to "mop up" and, of course, once it's in-step it'll stay in
step. 

The code used to generate it (but not the data) is here:
[python/day11part2_extracredit.py](https://github.com/henley-regatta/adventofcode2021/blob/main/python/day11part2_extracredit.py)

<video width="200" height="200" controls="controls">
    <source src="/assets/aoc2021_day11part2.webm" type="video/webm; codecs=vp9">
</video>


(If that doesn't work, Click [here](/assets/aoc2021_day11part2.webm) to view the video directly)

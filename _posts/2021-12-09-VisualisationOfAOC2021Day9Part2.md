---
layout: post
title:  "Visualisation of AOC 2021 Day 9, Part 2 answer"
date:   2021-12-09 15:30 +0000
categories: aoc2021
tags: aoc aoc2021 python png
---
# When a problem is just begging to be visualised...
Per previous updates, today's problem was begging to be visualised. Here is 
my attempt - the borders of the "basins" (all input values of 9) are shown in
black, the depths of each "basin" go progressively deeper blue as you get 
deeper, and the identified "low points" (which are used as the Part 1 answer,
and also the starting point for growing the basins which are the Part 2 answer),
are identified as bright red.

The input was 100x100 so I've upscaled by a factor of 10 for clarity, but this
web page scales that back to 50% until you clicky the actual picture.

![Visualisation of the Basins and Low Points on the heightmap for Day 9](/assets/aoc2021_day9part2.png "Colour plot of depths on the Heightmap for Day 9 with lowest points highlighted Red")
{:style="display: block; margin: auto; width: 50%;"}

Code to generate the solution and the visualisation is in the github repo at: [python/day9part2_extracredit.py](https://github.com/henley-regatta/adventofcode2021/blob/main/python/day9part2_extracredit.py)


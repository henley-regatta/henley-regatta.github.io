---
layout: post
title:  "Visualisation of AOC 2021 Day 15, Part 2 answer"
date:   2021-12-15 23:15 +0000
categories: aoc2021
tags: aoc aoc2021 python png
---

This one is something about safe routes through a cave, although given the 
cave "map" is algorithmically generated from a seed corner it all seems a bit 
unlikely. I've used colour to represent "danger" (darker=riskier) and the 
yellow line is the "optimal" route through the cave. Interestingly(?) there 
appear to be multiple routes with the same score; I had to adjust my route
finding criteria to ensure I was selecting the same one as the web page 
selects for the example data (not least of all because my many, many wrong runs
came to the right "number" for the example data but not via the right route).

The boundaries of the algorithmic expansion are more visible here than you'd
think, less so when viewed full-screen. 

![Visualisation of Day 15 Part2 Solution](/assets/aoc2021_day15part2_dijkstra.png "Final plot of optimal route from top-left to bottom-right calculated using the Dijkstra Algorithm")
{:style="display: block; margin: auto; width: 80%;"}

Unlike previous days, this time the visualisation's built in to the main 
problem solver; mainly because it takes so long to run (~1 hr) on the 
problem set that I was actually using the PNG output to assess the output 
quality. 


An example of a wrong-but-not-by-much answer plotted with the wrong assumptions
about path-movement is here:

![Visualisation of Wrong Answer for Day 15 Part 2](/assets/aoc2021_day15part2_downrightonly_dijkstra.png "Plot of wrong solution using faulty assumption that progress can only be down or right")
{:style="display: block; margin: auto; width: 80%;"}

Hopefully this shows why I was so convinced I was on the right track; the "score difference" for the two routes is 0.3% (difference of 9 in ~3000). 

(If I was using this for the real-world then I'd _definitely_ be using the 
simplified rules - the runtime for that "wrong" plot was ~5 seconds vs ~1 hr for the "correct" solution; definitely not worth it for the improvement)

The code used to generate it is here:
[python/day15part2.py](https://github.com/henley-regatta/adventofcode2021/blob/main/python/day15part2.py)


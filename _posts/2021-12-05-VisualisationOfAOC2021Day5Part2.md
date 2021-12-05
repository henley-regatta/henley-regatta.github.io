---
layout: post
title:  "Visualisation of AOC 2021 Day 5, Part 2 answer"
date:   2021-12-05 11:30 +0000
categories: aoc2021
tags: aoc aoc2021 python png
---

# Writing Graphics To Visualise Answers

I see visualising your answers is the Hot New Thing on [r/adventofcode](https://www.reddit.com/r/adventofcode/) and 
today's problem - Work out the intersections of a series of geothermal vents -
lent itself perfectly to this. Not least of all because the easiest way to 
actually come up with the answer you need was to, er, plot lines on a grid.

So here's my visualisation of the solution for my input data:

![Visualisation of Day5 Part2 Solution](/assets/aoc2021_day5part2_visualisation.png "Final plot of vent lines on grid")
{:style="display: block; margin: auto; width: 50%;"}

The code used to generate it (but not the source data, which is kinda-sorta
private to me) is in my answers github repo as [python/day5part2_extracredit.py](https://github.com/henley-regatta/adventofcode2021/blob/main/python/day5part2_extracredit.py)

### Update - Movies are Cool
After working out that appending to lists is slow but extending arrays is fast
I was able to make the image generation fast enough to generate a frame for
every vent being plotted. Which, using the magic of ffmpeg via the incantation:

`ffmpeg -framerate 25 -f image2 -i day5part2_%d.png -c:v libvpx-vp9 -pix_fmt yuva420p movie/day5part2.webm`

...leads to the glorious animation you see before you:

<video width="400" height="400" controls="controls">
    <source src="/assets/aoc2021_day5part2_movie.webm" type="video/webm; codecs=vp9">
</video>

(Embedded video support seems a bit wonky. Click [here](/assets/aoc2021_day5part2_movie.webm) to view the video directly)

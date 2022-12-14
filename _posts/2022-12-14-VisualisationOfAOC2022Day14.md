---
layout: post
title:  "Visualisation of AOC 2022 Day 14"
date:   2022-12-14 14:10 +0000
categories: aoc2022
tags: aoc aoc2022 golang png
---

Today's [Advent of Code problem](https://adventofcode.com/2022/day/14) was 
more visualisation-bait. The problem set was to first decode a (vertical)
map of rocks, and then to determine how sand falls through that landscape;
part 1 stops as soon as sand "hits the floor" (although the condition was 
stated differently. Part 2 had the code continue until the floor "fills up"
and propagates back to the entry point. 

#### My map decoded to: 
![Day14 Empty Rock Map Visualisation](/assets/aoc2022day14part2_0000.png "Starting visualisation of the decoded vertical rock map above our heads")
{:style="display: block; margin: auto; width: 80%;"}

#### And after I ran all the sand through it, it looked like:
![Day14 Final Map Visualisation](/assets/aoc2022day14part2_0321.png "Ending visualisation of the rock map with a sand cone stretching back to the entry point")
{:style="display: block; margin: auto; width: 80%;"}

#### And, naturally, this makes for a nice animation of sandfall:
<hr>
<center>
  <video width="745" height="369" controls="controls">
    <source src="/assets/aoc2022_day14part2.webm" type="video/webm; codecs=vp9">
  </video>
</center>
<hr>

The code used to generate these visualisations is here: [aoc2022/go/day14part12visualisation.go](https://github.com/henley-regatta/aoc2022/blob/master/go/day14part2_visualisation.go)

*** 

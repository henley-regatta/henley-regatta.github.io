---
layout: post
title:  "Am I Getting Less Fit Over Time?"
date:   2021-04-16 01:00 +0100
categories: dataanalysis
tags:  garmin data_analysis cycling fitness
---
# So, am I actually getting slower on the bike as I age?
I did a ride earlier, second this week. I don't exercise enough. That's
indisputable. And what's worse I'm inconsistent. So the *actual* answer to
the question posed above is: _Probably, but you'd not notice if you actually_
_rode some more you lazy slob_.

With that said, let's do some analysis.

## Data Is Everything
Yes, I log every ride. I have Garmin equipment to do so. In theory I can get
the raw data back out of Strava and look at it there, but who wants to be
bothered?

Garmin Connect has a nice "All Activities" page one can visit, scroll
through and download (as CSV) summary data for all activities. One quick
filter for "cycling" and, boomshanka. All of the following is based on my
last 100 rides.

## So am I getting slower over time?
Here's the average speed of my last 100 rides with a trend line:
![Average Speed of last 100 rides](https://docs.google.com/spreadsheets/d/e/2PACX-1vRsRa_HKeVxzpvNw6kNccj36rDQ3tE10z1FyAz9QeUa41gstAu5Ld2AZjatPsguySlogCqfEt36gVcF/pubchart?oid=1955255277&format=image)
See that little downward tilt of the trend line? Yes, I'm getting slower
over time. (...Well, a bit).

But that's not entirely accurate - maybe I'm riding further, or doing more
climbing, or some other factor is in play.

## OK slowcoach, so what about correcting for climbing.
So it makes a certain amount of sense that average speed will decrease if
you spend more time climbing than bombing along the flats[^1]. And I have
data on how much climbing I've done on a given ride. So I can adjust the
speeds to reflect this choosing "no climb" as zero-correction, with speed
being "corrected" upwards dependent on the average gradient[^2]:

![Normalised Average Speed of last 100 rides](https://docs.google.com/spreadsheets/d/e/2PACX-1vRsRa_HKeVxzpvNw6kNccj36rDQ3tE10z1FyAz9QeUa41gstAu5Ld2AZjatPsguySlogCqfEt36gVcF/pubchart?oid=326841156&format=image)
Oh. Nope, apparently I've still slowed down over the last couple of years (...Well, a bit).

But there's more ways to look at this.

## So am I slower over longer rides?
It makes sense now to think that perhaps if I'm doing _longer_ rides, my
average speed would decrease. Because I'm more tired and thus losing watts
and going slower as a result. What does the data say?

![Normalised Average Speed vs Ride Length](https://docs.google.com/spreadsheets/d/e/2PACX-1vRsRa_HKeVxzpvNw6kNccj36rDQ3tE10z1FyAz9QeUa41gstAu5Ld2AZjatPsguySlogCqfEt36gVcF/pubchart?oid=462404702&format=image)
Oh. That doesn't make much sense does it? Apparently I get _quicker_ the
further I go (...Well, a bit).

One explanation for this is that large cluster of rides between 20-30KM:
distance:

![Count of rides within distance buckets](https://docs.google.com/spreadsheets/d/e/2PACX-1vRsRa_HKeVxzpvNw6kNccj36rDQ3tE10z1FyAz9QeUa41gstAu5Ld2AZjatPsguySlogCqfEt36gVcF/pubchart?oid=918284232&format=image)
The shorter rides - up to about 30KM - are either "specials" like family
rides *or* they're _grabbed-at-short-notice_ / _not-got-much-time_ rides.

In the latter case I've got a tendency to substitute vertical climb for
horizontal distance to maximise the effect in a short time (...a bit like tonight).

## So am I slower on  steeper rides?
Yes. But not by as much as I thought it might be:
![Average Speed vs Average Climb Gradient](https://docs.google.com/spreadsheets/d/e/2PACX-1vRsRa_HKeVxzpvNw6kNccj36rDQ3tE10z1FyAz9QeUa41gstAu5Ld2AZjatPsguySlogCqfEt36gVcF/pubchart?oid=1262002674&format=image)
There's more of an observable effect here that points in the right (intuitively correct) direction.
As the average gradient goes _up_, average speed goes _down_.

This tells me something I already knew, but in raw factual form: I can't
climb.

## Why am I faster on longer rides?
It would be good to explain how my average speed _increases_ over ride
distance. Possibly I'm only tackling the longer rides at times when I'm
fitter (late-spring / summer as my "training regime" hits it's peak).
There's another possible explanation though, given we already know I get
faster the less climbing I do:

![Average Ride Gradient vs Ride Distance](https://docs.google.com/spreadsheets/d/e/2PACX-1vRsRa_HKeVxzpvNw6kNccj36rDQ3tE10z1FyAz9QeUa41gstAu5Ld2AZjatPsguySlogCqfEt36gVcF/pubchart?oid=1058403839&format=image)
The longer the bike ride, the shallower it is. I (probably) do
more _absolute_ climb on a longer ride but, crucially, as a fraction of it's
distance I'm climbing less, quite significantly, on longer rides.

Given where I live this isn't massively surprising: North and South of me are
hills, get over them (especially South which is where I'm likely to head
if the weather's good because who doesn't love seeing the sea on a sunny
day?) and head East/West and all of a sudden it's a flatter ride.

This is some evidence - a little - that my big worry about being too
slow to ride with my coast-based friends is overplayed. Because naturally
they're faster than me, they don't do as much climb and I've *proven* I get
slower as I do more climb.

The remaining question, of course, is whether the up-tick trend of speed vs
gradient is sufficient to outweigh the observed speed deficit...

***
[^1]: I'd love to also correct for things like "average windspeed" and temperature, but the Garmin summaries don't appear to have those included. More Research Required.
[^2]: It's hard to compare between the two charts as they look the same, but if you saw the raw data you'd see there _is_ a difference. Just not much of one, given the average climb gradients...

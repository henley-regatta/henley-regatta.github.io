---
layout: post
title:  "Am I Getting Less Fit Over Time?"
date:   2021-04-16 14:20 +0100
categories: dataanalysis
tags:  garmin data_analysis cycling fitness
---
Yes. Yes I am.

What with the whole one-thing-and-another going on, I'm just not getting out and doing any physical activity. I spend all my time here, on my favourite
chair, staring at my favourite set of light-emitting panels.

With that said, though, can I substantiate this in any way?

1. TOC
{:toc}

_Note_: This is version 2 of this post. I got more data, and did more
analysis, because I didn't like the conclusions of the first pass...
# I cycle. Am I actually getting slower on the bike as I age?
I did a ride earlier today[^3], my second this week. Sounds good but I'm
massively inconsistent in both quantity and quality of rides. Tonight's ride
data was depressing, though because I put in a maximum (perceived) effort
and the average speed came back slower than Monday's ride.

The rest of this post is intended to give insight into the fundamental
question: *Why?*.  

The actual answer is probably: _Normal variance, but you'd not notice if you actually rode some more you lazy slob. Also, why do you think ride speed is the metric to track?_

## Data Is Everything
_Obviously_, I log every ride. I have Garmin equipment to do so. In theory I
can get the raw data back out of Strava as per-ride GPX files and look at it
there, but who wants to be bothered[^4]?

Garmin Connect has a nice "All Activities" page one can visit, scroll through
and download, as a CSV, the summary data for all visible activities. This is
exactly what I'm looking for. A quick filter for "cycling" and, boomshanka[^5]

All of the following is based on the complete set of Cycling activities I've logged in Garmin Connect.

## Am I getting slower over time?
Here's the average speed of my rides with a trend line:
![Average Speed of all rides](https://docs.google.com/spreadsheets/d/e/2PACX-1vRsRa_HKeVxzpvNw6kNccj36rDQ3tE10z1FyAz9QeUa41gstAu5Ld2AZjatPsguySlogCqfEt36gVcF/pubchart?oid=1955255277&format=image)
A previous edition of this post would have shown the trend line decreasing
(slightly) over time. It was based on the last 100 rides not the full (170) set.

Apparently I'm _not_ getting slower. I'm getting ever-so-slightly faster, if
the linear regression line can be trusted (*spoiler*[^2]).

But that's an _observation_, it's not an _explanation_ - maybe I'm riding
further, or doing more climbing, or some other factor is in play.

## Am I slower on  steeper rides?
Yes. But not by as much as I thought it might be:
![Average Speed vs Average Climb Gradient](https://docs.google.com/spreadsheets/d/e/2PACX-1vRsRa_HKeVxzpvNw6kNccj36rDQ3tE10z1FyAz9QeUa41gstAu5Ld2AZjatPsguySlogCqfEt36gVcF/pubchart?oid=1262002674&format=image)
There's more of an observable effect here: As the average gradient goes _up_,
average speed goes _down_.

This tells me something I already knew, but in raw factual form: I can't
climb. It also gives me a possible correction-factor to look at to adjust
ride average speeds by average climb gradient.

(And _that_ analysis tells me I don't _actually_ have a strong enough
correlation - R-Squared of 4% - to  actually do anything legitimate with that.
Not going to stop me though, is it? )

## Am I slower over longer rides?
It makes sense to think that if I'm doing _longer_ rides, my
average speed would decrease. Because I'm more tired and thus losing watts
and going slower as a result. What does the data say?
![Normalised Average Speed vs Ride Distance](https://docs.google.com/spreadsheets/d/e/2PACX-1vRsRa_HKeVxzpvNw6kNccj36rDQ3tE10z1FyAz9QeUa41gstAu5Ld2AZjatPsguySlogCqfEt36gVcF/pubchart?oid=462404702&format=image)
Oh. That doesn't make much sense does it? Apparently I get _quicker_ the further
I go (...Well, a bit. Again, linear regression of speed/distance gives a low
R-squared of about 4%).

But there's some lovely fat deviations from that line both at the shorter and at the
longest distances. And there's plenty of data at the shorter, not much at the
longer end of that line:
![Ride Distance Frequency Distribution](https://docs.google.com/spreadsheets/d/e/2PACX-1vRsRa_HKeVxzpvNw6kNccj36rDQ3tE10z1FyAz9QeUa41gstAu5Ld2AZjatPsguySlogCqfEt36gVcF/pubchart?oid=918284232&format=image)
This chart feels about right - a 50-60KM ride at my average speed is about 1h 40
which is right on the "you have enough energy stored for about 90 minutes of
exercise" rule-of-thumb, so it makes sense that one of the big clusters is
there.

The larger speed deviation of rides in the ~25KM distance has two possible
explanations:
1. Some of these are "special" rides - out with the kids/family or other groups. Typically done at a more leisurely pace without the emphasis on performance.
1. If I'm solo and doing a ride of this distance it's because:
    1. I'm at the start of a training cycle (after a long break) and therefore very slow/unfit
    1. I'm doing a short one due to lack of time, and under those circumstances I _tend_ to favour finding a big hill to go up to maximise the utility of the ride[^6]

## Why am I faster on longer rides?
It would be good to explain how my average speed _increases_ over ride
distance.

I don't do enough _really_ long rides to come to any sort of rigorous
conclusions. I think, though, it's fair to say that if I'm tackling a ride of
that length I'm probably practicing for it and thus in better condition than for
the shorter rides. And I'm probably stopping mid-ride for refreshment to get over
the 90-minutes-of-energy thing.

I'm only tackling the longer rides at times when I'm
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

## OK slowcoach, so what about adjusting for climbing?
It makes a certain amount of sense that average speed will decrease if
you spend more time climbing than bombing along the flats[^1]. And I have
data on how much climbing I've done on a given ride. So I can adjust the
speeds to reflect this, applying a factor based on whether a given ride was steeper
or more shallow than the overall average gradient:
![Climb-Normalised Average Speed of all rides](https://docs.google.com/spreadsheets/d/e/2PACX-1vRsRa_HKeVxzpvNw6kNccj36rDQ3tE10z1FyAz9QeUa41gstAu5Ld2AZjatPsguySlogCqfEt36gVcF/pubchart?oid=326841156&format=image)
Oh, the trend's still there. That positive coefficient - +0.000212 - says that
over time my speed is increasing over time about twice as much as the raw data
would indicate  (*if* I trust the correlation. Which I really, really shouldn't:
r-squared is 1%).

Proper mathematicians would look at this and quite rightly mutter "Conclusions are Not Statistically Significant". And they'd be right.

But let's torture the data some more.

## What about if I adjust for distance?
I'm struggling to come up with a justification that works for this, but really
it's just an attempt to fudge the data to explain away the observation that "I go faster
the longer the ride". Or, if you prefer, attempt to adjust those shorter rides
that were probably taken with other people - what speed _should_ they have been
ridden at?
![Distance-Normalised Average Speed for all rides](https://docs.google.com/spreadsheets/d/e/2PACX-1vRsRa_HKeVxzpvNw6kNccj36rDQ3tE10z1FyAz9QeUa41gstAu5Ld2AZjatPsguySlogCqfEt36gVcF/pubchart?oid=1061005771&format=image)
So with this (_Extremely_ dubious) correction in place I can see I've still got
a positive trend line, indeed it's twice as steep as the one for climb-correction.
If you trust this correlation it's great news. But, again, you shouldn't - R-Squared
is only 3%

## Adjust for both! Adjust for both!
What happens if one adjusts for both distance and average gradient? Surely this
must be the _most accurate_ value we can get - it includes for all the factors we
can account for[^7]:
![Gradient and Distance-normalised Average Speed for all rides overlaid with the unadjusted speed for reference](https://docs.google.com/spreadsheets/d/e/2PACX-1vRsRa_HKeVxzpvNw6kNccj36rDQ3tE10z1FyAz9QeUa41gstAu5Ld2AZjatPsguySlogCqfEt36gVcF/pubchart?oid=1782141904&format=image)
(I've underlaid the un-adjusted speed here to visualise the difference the adjustment's making)

Oh. This time we've got a negative slope. I'm getting worse over time. Not by much:
decreasing about 1/2 as fast as I was increasing just looking at Climb, 1/4 as
fast as just looking at distance. But, the wrong way.

Having said that, the data's really telling me not to trust it. If my previous
correlations were _poor_, this one's *atrocious* - R-squared of 0.3% means "ignore
this finding" in anyone's books.

## Conclusions

At the core of my old man's unwarranted worrying is:
1. I'm getting worse over time, not better
1. As a consequence, and as I compare my performances to those of my coast-based
cycling friends, I'm becoming less and less able to keep up with them over time.

This data does not support the first concern, but _can not refute it_. Broadly
speaking I'm probably not getting any better or worse over time[^8]. The
confidence values for the trends I've discussed is so low as to be
basically chance. Any changes in the data reflect conditions on the day, most
important of which is how fit I am on a given ride - not a metric I can analyse
or compare against[^9]. I suspect there's an underlying actual conclusion that
if I *really* want to get better I'm going to have to be much more disciplined
about the frequency and intensity of my riding.

But: there *is* some evidence based on my own performance that I'm probably
not changing my _relative_ performance compared to my peers as much as I fear.
I can see that I'm doing more climbing than they are, and shorter rides. Assuming
the conclusions I drew for myself - the shorter and steeper the ride, the slower it
is for both metrics - apply to my friends then my recorded performance against
theirs probably, possibly means there's less of a gap than it appears.

I really ought to meet up with them for a long, flat ride. Preferably to a pub and
back.

## End.
{: .no_toc}

***
[^1]: I'd love to also correct for things like "average windspeed" and temperature, but the Garmin summaries don't appear to have those included. More Research Required.
[^2]: Nope, it really can't. That R-squared value is 0.2% - 0.2% of the variation being shown by the line is due to the data points. I'd be better off plotting the average speed and leaving it at that, frankly.
[^3]: Oops, yesterday. Should be in bed now really.
[^4]: Me, obviously. I already have analysis scripts for GPX files that would give me the summary stats I crave. But the night is late, and the bulk-export facility looks like a phaff. Another time, perhaps.
[^5]: Weirdly, though, you have to scroll all the way through the list on the Garmin page to load all data. The "Export to CSV" button is clearly client-side Javascript since it's only dumping the visible data, which is also clearly paged-on-request. Be nice if you didn't have to worry about that.
[^6]: As I did tonight with my after-work-before-dinner ride. 50% more climb in 12% more distance taking 25% longer to complete than Monday's ride.
[^7]: Again, I suspect quantifiable metrics that are _more_ relevant - temperature and average windspeeds - I don't have data for. And underneath everything is the Big Unquantifiable: *Just how much effort did I put into the ride that day as a fraction of my capacity?
[^8]: Although I am getting older and I'm in a phase of life where those effects will become more visible in the data.
[^9]: Hmm. I could probably synthesise a metric based on "number of rides in recent past" which may show something for this?

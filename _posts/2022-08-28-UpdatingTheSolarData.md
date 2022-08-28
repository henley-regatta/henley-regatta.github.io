--- 
layout: post
title:   "Updating the Solar Generation Statistics after 2 Years Use"
date:   2022-08-28 19:00:00 +0100
categories: dataanalysis
tags: dataanalysis solar nerdstats
--- 
Just over two years ago we had a Solar PV system put in, panels and a battery. I wrote a [very enthusiastic but mostly speculative](https://henleyregatta.blogspot.com/2020/08/solar-power-is-great-at-least-as-toy.html) post on how excited I was and how I thought we might see some savings.

Time has passed, and thanks to my early work I've captured data for every day in the intervening period. So it's time to update my predictions based on Observed Reality[^1]....

## A Note On Changing Conditions
The world's a very different place, certainly here in the UK, since we had the system installed. In particular due to Market Circumstances, the retail cost of electricity has skyrocketed. Two years ago the average rate was _around_ 15p/kWh. Now, the Cap Rate has gone up to 54p/kWh, or nearly 4 times higher. This has had an _enormous_ effect on the (economic) effectiveness of Solar PV installations. 

## Data Analysed
Two years ago I rather speculatively guessed we'd generate about half of the electricity we use domestically. Given that I was going on 6 weeks of actual data, at a time when the single biggest consumer of electricity - the car - wasn't being used due to COVID, I'm actually surprised how _accurate_ that guess was.

The 793 days worth of data I've gathered tell me that we've generated **53.6%** of the electricity we've consumed in total.

## Economic Impacts
My initial, guesstimated, payback time for the system was "about 20 years" although I thought that was optimistic. With the benefit of a couple of years of actual data, calculations show that actually that was about right _assuming market conditions remained the same_. But they haven't, very much not so.

Everyone's situation will be different so time-to-payoff will vary greatly. With the tariff I've managed to get onto for at least another year, and with the huge increase in the cap rate, my time-to-payoff has gone down to **5 years**. 

Three important factors should be considered in assessing this number:
  * As previously noted, adding a battery to the system doubled the installation costs. Without one, time-to-payoff would be half.
  * _However_ I was only able to get onto the (extremely beneficial) tariff I'm on _because_ I have generation and storage. Generation-only (no battery) would be nowhere near as economically effective.
    * Although far more of the generated energy would be exported, it would likely be at a much reduced rate and so offset less of the system costs. And possibly the import rate wouldn't be anywhere near as beneficial either.
  * The huge decrease in time-to-payoff is because of enormous discrepency between the "cap rate" and the import rate I'm _actually_ paying. It is not realistic to expect this discrepency to remain as large for the total payoff period.

A more realistic estimate of actual time-to-payoff would be closer to 10 years given best extrapolation of the market conditions likely to exist over that timescale. 

## Generation vs Export
I have chosen to ignore the impact of any electricity we've _exported_ to the grid as, although I do get paid for it, it's a small fraction of the total.
 
As noted in the initial analysis, having a big battery onsite "soaks up" most of the generated electricity. On a nice sunny late-spring/summer day we'll probably generate about 50% more electricity than we'll use (over the 24 hour period) so you'd expect we'd have some significant exports and, occasionally, we do. But the weather being the weather we're just as likely to consume that excess (via the battery) the following day, because except for rarer short periods two completely-sunny days aren't very common.

As a result of this, calculations show we export just under 5% of the total PV generation back to the grid. Or, to think of it another way, our Solar PV generation is about 5% over-sized for our storage capacity. Either way, what we're showing is that there's little room for growth in our _consumption_ without drawing more from the grid.

## Generation Efficiencies

*(This is a little more "statistical" than accurate, because I'm not directly accounting for time of year but making some assumptions)*


We have a 3.15kW SolarPV system installed across 10 panels, arranged as 7 mostly-South facing, and 3 mostly-East facing. What's interesting about (obsessively?) watching generation through the day is that on a warm summer's day the system will never go above about 2.2kW; i.e. about 70% of theoretical capacity. _However_, earlier in the year when the temperatures are not as high, we'll see short (<5 minutes) "spikes" up to 3/3.1kW. 
This tracks with some of the data out there about solar conversion efficiencies as a function of ambient temperature (*i.e.* PV panels are more efficient when cold). 
It does make me wonder whether fitting some sort of under-panel cooling system might improve generation capability; ideally it'd be tied in to the household hot-water system for double benefits....

Ignoring this pointless speculation, our average generation through the year is 8.7kWh/day. Our "generating days"[^2] average 11.2 hours across the year. This gives an effective average generation power of 777W, or about 25% of the installed capacity. 

Our "best day" generated more than double that but occured on a longer summer's day (20.25kWh on an assumed 15.9 generating-hour day). That managed 41.5% of rated capacity across the day. 

It's harder to know how to translate these figures to a more general case because they're critically dependent on a huge range of factors, not least of which is the specific arrangement of panels on a roof and their alignment. Still, averaging a quarter of installed capacity looks like it ought to be a useful rule-of-thumb for someone.

## Accounting for the Car
A significant complication to my sums is separating out what's "household" consumption and what's due to the car. And _that_ isn't helped by the car topping itself off even when it's not being used...

My mileage is still a fraction of what it was pre-pandemic. In both 2020 and 2021 I estimate we did about 25% of the pre-pandemic mileage; so far 2022 is likely to be closer to 50-60%.  

There are two sets of calculations I can do. The first is based on my recollections of mileage, my observations of car efficiency, and estimations of home charging percentage. This gives me a figure of about 8% of total household consumption going to the car. If we return to pre-pandemic levels of driving that rises to 24%

The second form of calculation I can do uses actual data from the car, all be it restricted to the last 31 days. This is accurate in terms of charge-stats, but misleading for a longer-term analysis (it falls in a period where we've made 2 long-range trips involving significant off-house charging). This gives me about 30% of total household consumption going to the car. 

It would seem reasonable to extrapolate a normal year's car-related consumption going from the current ~8% up to something like 15-20%; since we know we're at the limit of generation capacity that will therefore increase our grid electric costs by about 10%. This will increase time-to-payoff.

### Other Factors Affecting Time To Payoff
The car is  _not_ the biggest unknown in the estimations of time-to-payoff, by the way, since we can expect over that timescale to shift our energy usage away from Gas, which we currently use for heating and cooking, to more electricity[^3]. As noted, we have hit our headroom for generation with little to export which means any increase in electricity consumption will have to come from the grid. This will throw off all the calculations, possibly quite considerably...

***
[^1]: As with the initial analysis, I'm going to draw a fig-leaf over actual figures and use relative / indicative numbers. Partly because I'm shy, partly because relative values are more portable to other use-cases without having to worry about the specifics of our house, energy supplier, or market conditions.
[^2]: Based on my calculations of day length calculated as the first/last times the panels generate >10W as a marker for sunup/sundown. Bears only a passing relation to the actual astronomical sunrise/sunset times but useful nonetheless.
[^3]: Gas prices have gone up a lot too, but not - _currently_ - as much as electric. However, the longer-term outlook is worse for Gas because it can only become scarcer and more expensive. We'd hope, though, that renewable / green Electric generation capacity is increasing and should maintain or (ideally!) decrease the costs. 
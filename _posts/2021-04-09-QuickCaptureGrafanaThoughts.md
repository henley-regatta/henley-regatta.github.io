---
layout: post
title:  "Grafana - Some Incomplete/In Progress Thoughts"
date:   2021-04-09 17:30 +0100
categories: incomplete
tags: incomplete grafana experiences
---
I have an actual business need to fully get to grips with [Grafana](https://grafana.com) and
I also have several home use-cases that are best served by having some nice
graphical dashboards available. So I've been spending some time trying to get to
grips with it and understand how best to use it.

## Caveat Lector
For context: I've been building dashboards or other reporting tools for the
interpretation of metric data for most of my career, across a range of tools,
technologies and platforms too numerous to mention. I don't think this makes me
an _expert_ at this, but I do have some experience that affects my
judgement and impartiality. I'm bound to have some _we've never done
it like **this** before_ attitudes affecting me.

With that said, right off the bat I have to say Grafana is impressive at making
the simple stuff easy. It's got a very low bar to entry, far lower than most of the tools
I've used over the years.

## Data is the Hard Part
I'm not going to discuss the hardest / most complicated issues involved here - those of
identifying, quantifying, gathering, processing and storing the data on which dashboards
depend. Whole books could and probably should be written on this subject.

But, it cannot be stressed enough: You can't visualise what you haven't got (and
cannot synthesise). No Dashboard tool can pull graphs out of thin air. To a first
approximation, your experience with a Dashboard tool, and Grafana in particular,
will depend on what data you've got to report on, and how it's available to you.

The majority of the comments I will make here therefore apply to _my_ experience
using _my_ combination of Data Source + Visualisation Tool. It's not fair to
review, critique, or compare them individually. Your mileage will most assuredly
vary.

Nearly all of my work with Grafana so far has been with using Time-series
data from an Influx (1.8, so `InfluxQL`) data source. Which is pretty good,
pretty flexible but limited in ways that directly affect my dashboarding
experience with Grafana. I'd highlight both the inability to process time as an independent variable,
and the inability to perform "join" operations on tables as being particular
restrictions in my capabilities here.

## Low Barrier To Entry
Grafana is properly lightweight. I've used plenty of "professional" visualisation
toolsets that demand huge server clusters to run on, take days or weeks to setup, and have
large, complicated (and brittle) infrastructures that need continuous care and attention.
This is the opposite.

Grafana installation is lightweight and simple. After toying with it in a WSL-2 container
on a Windows install (via a `snap` package), my home-use "Production Install" is hosted
on a Raspberry Pi 3b which is also being used as a print server, an MQTT server and I
see it's also still running a custom Node.js-based scheduler I wrote a few years back too.

The [installation instructions](https://grafana.com/tutorials/install-grafana-on-raspberry-pi/)
for an already-running Raspbian-based Pi are simple and quick and take about 10
minutes to complete. The server itself is _very_ lightweight (idling it's taking up 5% of the
Pi 3's 1GB RAM and just about no CPU, and it's config/cache/database are trivially small on disk - a few
MB to get started). I'm sure it's possible to build installs that consume serious resources
but it's possible to do some serious work - a few 10's of dashboards concurrently hosted - on
such puny resources.

(Yes, it's also available on the containerised platform of your choice. If you've
got a cluster available to you, a Grafana container here or there isn't going to
add much to the footprint of your cluster).

There is, of course, a reason for this: Most of the heavy lifting is performed
by either the browser client rendering the dashboards, or the data source gathering,
consolidating, and processing the data to be sent. And not the Grafana server itself.
This approach would have drawbacks in terms of scalability if you had lots of users all viewing the
same graphs and data sources simultaneously... which is the assumption many of the
"professional" toolsets make that drives their large infrastructure.

However, my own experience is that this isn't a particularly valid assumption to make.
When there's lots of eyeballs viewing metric data, it's normally via some large projected
 shared console (thus queried and displayed once). And  if there's lots of simultaneous users they're rendering data from
different sources (with a queries->displays ratio approaching one, thus hard to cache/optimise).

But more often than not, there just aren't that many simultaneous
users - even in very large companies it's rare to find more than 10 people with a
need to access complex data in anything approaching realtime[^1]. So having a tool
that doesn't attempt to optimise the query loads (because these tend not to be
repeated[^2]) and pushes the display load down onto the machine of the user actually using it makes
sense.

## Making the tradeoff explicit
Just for clarity: The strength of Grafana - lightweight therefore fast and easy
to use - is also at the root of it's core limitations. Grafana can only present
the data it can retrieve. It cannot _process_ that data. If your datasource doesn't
provide the number to Grafana (and _as a number_) then you're not going to be able
to come up with it in Grafana. I'm aware of Transforms and using
"hidden" panels as data sources but as far as I can tell, these are great for
summarising and simplifying data but not so hot on synthesising new values.

The practical effect of this is that more load is placed on the datasource than
on the Grafana server itself. An example: I have time-series data covering my
energy consumption at various granularity levels. And I know what my current
and historical billing rates are. It doesn't make sense to store my consumption
(kWh of electricity) as a financial cost (what if the rate changed mid-period? I
might only find out weeks later when my next bill comes in...) but it _does_ make
sense to visualise as such. In order to do so with an Influx back end (..and one
old enough to require `InfluxQL` rather than `Flux` so I can't do table joins
on), I need a way of combining the cost-per-kWh with the kWh consumed in a given
period. As far as I can see the best way of doing this is having that variable data -
`costPerkWh` - encoded as a variable on a Grafana Dashboard page. All well
and good but what _actually_ happens when I try and graph it is that this value
is encoded into the query that's sent to the Influx data-source so that _it_ can
do the processing on every value before it's sent to the client (browser).

Is this bad? Not necessarily but it's a performance factor you'd better allow for:
Does your data source back-end have sufficient performance to be performing
display calculations for you as well as processing storage, filtering, aggregation
etc operations? How well does this scale to huge-scale data sources? Can you write
your queries to perform those calculations in an outer loop (as you'd like) or
does the query language constrain you to transforming source data before filtering (expensive).

## Experential Glitches
I just want to document here things I've observed that I think I shouldn't have seen, or
that are restrictions I'm struggling to believe are The Right Way To Do It. These
aren't so much "bug reports waiting to happen" as things that highlight I don't
understand _everything_ about this yet.

1. Random session sign-outs. According to the values in `grafana.ini`, once I've
logged in to a session on a browser, that session should persist for weeks (and yes I'm using _admin_ for everything because
I'm single-user but I have changed the password...). Actually I've changed it
to months (`login_maximum_lifetime_duration = 2M` and `login_maximum_inactive_lifetime_duration = 14d`
means I should be able to go on a holiday and come back and still get straight to a dashboard).
But my experience is that I'm getting thrown back to the login screen at least once a day,
usually more. The Settings page shows I've got multiple sessions for the user active, but the
session cookie has the expected expiration duration. I don't get it.
1. Why can't I plot that number you're showing me in a legend? Many of my visualisations
would be improved by plotting boundary lines - the maximum, the average value found
for a series. If you've selected to have a legend you can add these values to
that legend (very handy) but...that's all. There's no way of adding _from within
Grafana_ dynamically-calculated threshold or boundary lines. Static ones with fixed
values? Sure, no problem and very pretty they look. But that average value in the
legend plotted as a simple dotted line across the graph? Nope.
    * The solution to this one is as obvious as it's irritating: get the data source
    to calculate the average at a point in time and plot that as an additional series.
    Clever manipulation of the `$__interval` value in a second query for the same panel
    lets one do things like plot longer-term averages alongside raw data which looks
    really good **but** as with everything else it's down to what your Data Source
    can do _and_ the processing load goes onto it, not Grafana or your Client
    as you might hope.
1. The defaults suck, especially for single-stat values. The standard workflow
for building a panel builds queries that retrieve _all_ the data for a given configured
time window (possibly years!), probably aggregates it into `$__interval` chunks and then
throws out all but the last value to plot on a gauge. It's up to you, the user, to
remember to re-write that query to remove the aggregation and grouping criteria
and just do a `LAST(x)` for the one metric you're interested in.
1. Window design. I get that Grafana is supposed to be your palette of a wide
range of dashboards, that's great. But 90% of use-cases are going to want to default
a user, on login, to their _preferred_ page. I know I do. That's fine, but you -
not Grafana, _you the dashboard designer_ - have to remember to configure those
"home" dashboards with links to the other pages that user may want to visit.
    * Why make a deal about this? Because the inbuilt navigation is horrible. If
    a user has a "home dashboard" configured then clicking the "home" icon - the
    logo top-left - takes you there, instead of to a list of dashboards. As does the
    Dashboards->Home navigation. The alternative appears to be Dashboards->Manage,
    but that takes you straight into what looks like a file explorer interface,
    and to add insult to injury if you've organised dashboards into folders they're
    closed by default so you can't actually see _any_ dashboards to click on until
    you work out it's a folder, the select-boxes to the left aren't relevant, and
    you can click on them to open them and show dashboards underneath.
    * The user-friendly(?) work-around is to make sure a user's home Dashboard
    has _Links_ configured, and that within that there is a _dashboard link_ to
    the set of dashboards you'd like them to see. And even this is confusing,  
    because you've gone to the trouble of grouping your dashboards into Folders,
    but you can't filter these dashboard links by that, no no instead you've got
    to "tag" each dashboard built and then you can filter by that instead. **WHY?**
    * God help you if you forgot to do this on a large environment and just allowed
    "view all" because the top of the dashboard page becomes just a l-o-n-g list
    of all the created dashboards. Things get better - a bit - if you rememebered
    to tick the "Show as dropdown" option, but even then there's borkage: From
    the list displayed you'll get not only the dashboards available _but also the
    folders as separate clickable items_. And they're not distinguished. So when
    you click you've got no idea whether you're going to be taken to an actual
    new dashboard page, or thrown back out to a (filtered) view of that file-explorer
    like dashboard organiser page[^3]



***
[^1]: One reasonable objection to this assertion is "But everyone needs to know the current value of X" where X = company stock price, or currently open trouble tickets, or telephone wait time or some other globally-relevant performance indicator. A rebuttal to this objection is that if the actualcurrent value is important, it'll be embedded as a raw number on whatever common info mechanism the organisation uses (typically a web homepage). It's rare that more complex visualisations of core operational metrics are required and even rarer that there's no better way of building and visualising them for a wide concurrent audience than using a general-purpose dashboarding tool!
[^2]: We need to think about automatic dashboard refresh intervals here, though, I think...
[^3]: Actually I don't know what I'm asking for here except "better, more discoverable navigation". All I can tell you is that if word-count translates to irritation level with something, this is right up there as my number one peeve with Grafana.

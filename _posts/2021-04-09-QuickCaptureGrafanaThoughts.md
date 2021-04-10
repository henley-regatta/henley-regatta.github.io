---
layout: post
title:  "Grafana - Some Incomplete/In Progress Thoughts"
date:   2021-04-09 17:30 +0100
categories: incomplete
tags: incomplete grafana experiences
---
I have an actual business need to fully get to grips with
[Grafana](https://grafana.com) and I also have several home use-cases that are
best served by having some nice graphical dashboards available. So I've been
spending some time trying to get to grips with it and understand how best to
use it. Here are some thoughts that no doubt in time I'll come to disown.

## Caveat Lector
For context: I've been building dashboards or other reporting tools for the
interpretation of metric data for most of my career, across a range of tools,
technologies and platforms too numerous to mention. I say this not to claim I'm 
some sort of  _expert_ at this, but to warn that this experience will affect my
judgement and impartiality - I'm bound to have some _we've never done
it like **this** before_ attitudes affecting how I see this..

With that said, the summary of this entire massive post is: Grafana impresses
me with it's ability to make the simple stuff easy and quick to do. If you have
a need to visualise numeric data, spinning up a quick instance should be a
fairly obvious and natural first move. But just because the simple stuff is
easy, don't expect that you've solved _all_ your dashboarding problems in one
fell swoop. There are complications and considerations to be made...

## Data is the Hard Part

I'm not going to discuss the hardest / most complicated issues involved here -
those of identifying, quantifying, gathering, processing and storing the data
on which dashboards depend. Whole books could and probably should be written on
this subject.

This one fact cannot be over-stressed: You can't visualise what you haven't got
(or cannot synthesise from what you've got). No Dashboard tool can pull graphs
out of thin air. To a first approximation, your experience with a Dashboard
tool, and Grafana in particular, will depend on what data you've got to report
on, and how it's made available to you.

The majority of the comments I will make here therefore apply to _my_
experience using _my_ combination of Data Source + Visualisation Tool. It's not
possible to critique or compare them individually. Your mileage will most
assuredly vary.

Nearly all of my work with Grafana so far has been with using Time-series data
from an Influx (1.8, so `InfluxQL`) data source. Which is pretty good, pretty
flexible but limited in ways that directly affect my dashboarding experience
with Grafana. I'd highlight both the inability to process time as an
independent variable, and the inability to perform "join" operations on tables
as being particular restrictions in my capabilities here.

## Low Barrier To Entry

Grafana is properly lightweight. I've used plenty of "professional"
visualisation toolsets that demand huge server clusters to run on, take days or
weeks to setup, and have large, complicated (and, often, brittle)
infrastructures that need continuous care and attention.  Grafana is the
opposite of this.

Grafana installation is lightweight and simple. After toying with it in a WSL-2
Ubuntu VM on a Windows install (via a `snap` package), my home-use "Production
Install" is hosted on a Raspberry Pi 3b which is also being used as a print
server, an MQTT server and I see it's also still running a custom Node.js-based
scheduler I wrote a few years back too.

The [installation
instructions](https://grafana.com/tutorials/install-grafana-on-raspberry-pi/)
for an already-running Raspbian-based Pi are simple and quick and take about 10
minutes to complete. The server itself is _very_ lightweight (idling it's
taking up 5% of the Pi 3's 1GB RAM and just about no CPU, and it's
config/cache/database are trivially small on disk - a few MB to get started).
I'm sure it's possible to build installs that consume serious resources but
it's demonstrably capable of doing some serious work - a few 10's of even quite
complex dashboards concurrently hosted - with such puny resources.

(And, yes, it's also available on the containerised platform of your choice. If
you've got a cluster available to you, a Grafana container here or there isn't
going to add much to the footprint).

I'm guessing the reason for this is that most of the heavy lifting is performed
by either the browser client rendering the dashboards, or the data source
gathering, consolidating, and processing the data to be sent. Not by the
Grafana server itself.  

This approach would have drawbacks in terms of scalability if you had lots of
users all viewing the same graphs and data sources simultaneously. This is
the assumption many of the "professional" toolsets appear to make that's one
big driver for their complexity and large footprint.

My own experience is that this isn't a particularly valid assumption
to make. Common use-cases for visualising metric data include:

1. Lots of "eyeballs" watching a set of common data visualisations to guide
their work. This is the classic "Operations Centre" view. Which tends to be
driven by having a single view of a set of visualisations projected onto a
wall.
    * The characteristic here being that dashboards are generated once and viewed (effectively) once; it's not a scenario where complex data-query/transform/caching mechanisms will gain much improvement.

2. Very large numbers of "eyeballs" tracking one or two critical metrics. This
is the "Need to know how many calls are in the call centre queue" or "What's
the company stock price" use-case. 
    * If this isn't being met by a projected large-screen display (reducing this to a special case of the first scenario), it's normally met by directly embedding the one or two critical metrics directly into whatever primary interface is being used; interpretation/visualisation of these metrics is so basic a Dashboarding system of _any_ kind would be massive overkill

3. Relatively small numbers of "eyeballs" (normally in the ~10 at a time range,
even in large organisations) generating more complex visualisations of related
but not identical data. This is the specialists-working-a-problem or perhaps a
many-workers-managing-many-instances (e.g. stock fund managers) use case.
    * In this case, although possibly the dashboards are repeated in terms of layout and presentation, the data queries used differ in detail ("I need to see Linux performance for machines A-Z, She's looking at the same data for machines 01-99"). Although there's scope for caching of metadata - displays, layouts etc - the actual data's not shared so complex infrastructure to cache and re-use queried data isn't going to achieve anything.

None of these use-cases gains much from having a Dashboarding/Reporting
infrastructure in place that's able to cache or proxy actual data queries. This
would only be an appropriate optimisation if the system was required to
continually generate and present identical data and dashboards to a number of
different clients[^1]. 

If you find yourself in that situation, you'll want to take into consideration
the fact that Grafana's default _modus operandi_ might not scale to your needs
or at least be efficient at that scale. I'm willing to bet very few people find
themselves in that situation, though.

## Making the tradeoff explicit

Just for clarity: The strengths of Grafana - lightweight therefore fast, and
easy to use to generate graphic visualisations - are also at the root of it's
core limitations. 

Grafana can only present the data it can retrieve. It cannot _process_ that
data. If your datasource doesn't provide the number to Grafana (and _as a
number_) then you're not going to be able to come up with it in Grafana. I'm
aware of Transforms and using "hidden" panels as data sources but as far as I
can tell, these are great for summarising and simplifying data but not so hot
on synthesising new values.

The practical (infrastructure; who cares about the client anyway?) effect of
this is that more load is placed on the datasource than on the Grafana server
itself. 

An example: I have time-series data covering my energy consumption at various
granularity levels. I also know what my current and historical billing rates
are. But it doesn't make sense to store my consumption (kWh of electricity) as
a financial cost (what if the rate changed mid-period? I might only find out
weeks later when my next bill comes in...) . It _does_ make sense to visualise
the data as "financial" though, because that's the number that I'm most
interested in understanding. 

In order to do so with an Influx back end (..and one old enough to require
`InfluxQL` rather than `Flux` so I can't do table joins), I need a way of
combining the cost-per-kWh with the kWh consumed in a given period.  As far as
I can see the best way of doing this is having that variable data -
`costPerkWh` - encoded as a variable on a Grafana Dashboard page. I can then
use this variable by putting it into the Grafana panel query to transform "kWh consumed" into "cost of electricity".  

This works well, but it's worth understanding that what _actually_ happens when
I try and graph it is that this value is encoded *into the source of the query*
that's sent to the Influx data-source so that _it_ can do the processing on
every value before it's sent to the client (browser).

Is this bad? Not necessarily but it's a performance factor you'd better allow
for: 
* Does your data source back-end have sufficient performance to be performing display calculations for you as well as processing normal back-end storage, filtering, aggregation etc operations? 
* How well does this scale to very large data sources? In part this'll be driven by:
* Can you write your queries to perform those calculations in an outer loop (as you'd like) or does the query language constrain you to transforming source data before filtering (expensive).
    * In my example, it's the difference between transforming _every_ kWh data-point into cost _before_ aggregation/filtering/reduction (at **O(n)** performance) vs doing all that processing and _then_ converting the (singular) result to cost (at **O(1)** performance). Because Grafana can't do the post-query processing to do this efficiently, and because of InfluxQL design limitations, I'm often forced to do the former. 

## Experential Glitches
I'm going to document here things I've observed that I think I shouldn't have
seen, or that are restrictions I'm struggling to believe are The Right Way To
Do It In Grafana. As such, these aren't so much "bug reports waiting to happen"
as things that highlight I don't understand _everything_ about this yet.

1. Random session sign-outs. According to the values in `grafana.ini`, once
I've logged in to a session on a browser, that session should persist for weeks. But my experience is that I'm getting browser refreshes that throw me back out to the login page at least once a day, usually several times.
    * Is this because I'm using _admin_ for everything because I'm single-user? I know there's special behaviour for default password but I've changed it.
    * Actually I've changed it to months (`login_maximum_lifetime_duration = 2M` and `login_maximum_inactive_lifetime_duration = 14d` means I should be able to go on a holiday and come back and still get straight to a dashboard).
    * The Settings page for the user shows I've got multiple sessions active, but this behaviour isn't changed if I manually remove the other sessions (both on the in-use browser and on other browsers on other machines I've been testing on)
    * Inspecting the app, I can see that the configured session cookie has the expected expiration duration. I don't get why it's happening.

2. Why can't I plot that number you're showing me in a legend? Many of my
visualisations would be improved by plotting boundary lines - the maximum, the
average value found for a series. If you've selected to have a legend you can
add these values to that legend (very handy) but...that's all. There's no way
of adding _from within Grafana_ dynamically-calculated threshold or boundary
lines. Static ones with fixed values? Sure, no problem and very pretty they
look. But that average value in the legend plotted as a simple dotted line
across the graph? Nope.
    * The solution to this one is as obvious as it's irritating: get the data source to calculate the average at suitable points in time and plot that as an additional series.
    * But there again, clever manipulation of the `$__interval` value in a second query for the same panel lets one do things like plot longer-term averages alongside raw data which looks really good **but** as with everything else it's down to what your Data Source can do _and_ the processing load goes onto it, not Grafana or your Client as you might hope.

1. The defaults suck, especially for single-stat values. The standard (guided?)
workflow for building a panel builds queries that retrieve _all_ the data for a
given configured time window (possibly years!), probably aggregates it into
`$__interval` chunks and then throws out all but the last value to plot on a
gauge. 
    * It's up to you, the dashboard designer, to remember to re-write that query yourself to remove the aggregation, grouping and probably time-frame selection criteria and just do a `LAST(x)` (or `AVG(x)` etc) for the one metric you're interested in.

1. Navigation design. I get that Grafana is supposed to be your palette of a
wide range of dashboards, that's great. But 90% of use-cases are going to want
to default a user, on login, to their _preferred_ page. I know I do. That's
fine, but you - not Grafana, _you the dashboard designer_ - have to remember to
configure those "home" dashboards with links to the other pages that user may
want to visit.
    * Why make a big deal about this? Because the inbuilt navigation is horrible. If a user has a "home dashboard" configured then clicking the "home" icon - the logo top-left - takes you there, instead of to a list of dashboards. As does the Dashboards->Home navigation. The alternative appears to be Dashboards->Manage, but that takes you straight into what looks like a file explorer interface, and to add insult to injury if you've organised dashboards into folders they're closed by default so you can't actually see _any_ dashboards to click on until you work out it's a folder, the select-boxes to the left aren't relevant, and you can click on them to open them and show dashboards underneath.
    * The user-friendly(?) work-around is to make sure a user's home Dashboard has _Links_ configured, and that within that there is a _dashboard link_ to the set of dashboards you'd like them to see. And even this is confusing,  because you've gone to the trouble of grouping your dashboards into Folders, but you can't filter these dashboard links by that, no no instead you've got to "tag" each dashboard built and then you can filter by that instead. **WHY?**
    * God help you if you forgot to do this on a large environment and just allowed "view all" because the top of the dashboard page becomes just a l-o-n-g list of all the created dashboards. Things get better - a bit - if you rememebered to tick the "Show as dropdown" option, but even then there's borkage: From the list displayed you'll get not only the dashboards available _but also the folders as separate clickable items_. And they're not distinguished. So when you click you've got no idea whether you're going to be taken to an actual new dashboard page, or thrown back out to a (filtered) view of that file-explorer like dashboard organiser page[^2]
    * Also worth noting is that building dashboard navigation this way completely negates the point of the built-in "favourite" mechanism. With login-to-homescren behaviour, favouriting a Dashboard bubbles it to the top of the list of dashboards on that page, making it easier to find. But when an actual Dashboard is set as home, there's zero benefit of marking as favourite. It doesn't affect ordering in the "dropdown" or the spew-over-the-page mode of handling Dashboard links and (as far as I can tell) there's no way to get to the list-of-dashboards-as-home page that does use that mechanism.

1. The pain of dynamic time-window setting. And this is firmly in the "It's a Grafana+Influx thing" camp. In my example above I'd like to be able to set time-windows that match my billing periods, which don't align to month start/end and can change by a day or two each period. Ideally, I'd use the timestamp of an entry in the billing details (normally the last one) to say "Show me the consumption since X" to get an idea of how much my _next_ bill is likely to be. I cannot find any way to do this, though, because both Grafana and Influx treat Timestamps on time-series data as "implicit" - required in order to locate the point/value being plotted but in no way externally accessible. 
    * Unless I'm missing something. Which I might well be. I cannot find a way either in a Panel query or in a Variable query to get Influx to spit out a numeric value representing a date that equates to "the timestamp of the last data row from X measurement". 

***
[^1]: We need to think about automatic dashboard refresh intervals here, though, I think...
[^2]: Actually I don't know what I'm asking for here except "better, more discoverable navigation". All I can tell you is that if word-count translates to irritation level with something, this is right up there as my number one peeve with Grafana.

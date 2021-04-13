---
layout: post
title:  "Scratching the Grafana Timerange Itch"
date:   2021-04-12 17:30 +0100
categories: grafana
tags: incomplete grafana experiences
---
I would like to be able to graph or visualise data in Grafana according to
custom time-ranges. Specifically I have "billing period" data that goes broadly
mid-month to mid-month; this doesn't align with any of Grafana's options for
selecting time ranges.

## Things I know don't work.
If you've read [my previous attempts to do this]({% post_url 2021-04-09-QuickCaptureGrafanaThoughts %})
you'll see I've thought of a number of strategies to achieve this.  What I
thought would be most promising was the use of _Dashboard Variables_ to cause
parameters to be selectable on a page for the timerange required. Couple of issues
prevent this:

1. `InfluxQL` doesn't allow the _sole_ return of a timestamp from a query. You have to do `select time, metric FROM measurement`
and then parse the date out. Grafana Query Variables expect a single list of returned values and
there appears to be no way to tell it to give me the timestamps, not the metric value.
1. Not 100% sure I can stick to this but I _suspect_ I'm unable to tie different
panel (queries) to two _related_ values. Specifically I have associated tuples of
"Date" and "Meter Reading"; most values need the Date but at least one ("Estimated current reading")
also needs the reading value to calculate an offset. Although I can define 2
variables for the Dashboard (equating to "Date" and "Reading"), I can't work
out how to link them so that selecting one automatically selects the other associate.
1. Similarly, I thought of implementing this using [Scripted Dashboards](https://grafana.com/docs/grafana/latest/dashboards/scripted-dashboards/) which
essentially work by wrapping up a page definition in a javascript generator; in
my case I'd use it to specify rows. The challenges with this are:
    1. It's a [Deprecated Feature](https://github.com/grafana/grafana/issues/24059)
    1. You've changed the problem into working out how to do a Influx query from
    "native" javascript rather than the dashboard, _and_ parse the results by hand.

## Something that Sort-Of Works: A custom Plugin - JSON API
Most of the plugins expect a complex query language in order to get data; the
[JSON API Data Source for Grafana](https://marcus.se.net/grafana-json-datasource/installation)
will just grab whatever's at a URL. This suits me as I already have a raw
data source (that actually feeds my InfluxDB's `meterreadings` measurement) available
at a local URL.

The next issue is that the JSON is _very_ sparse; it's a series of key:value pairs
where the key is the date, the value the meter reading:

{% highlight json %}
{ "2021-03-15": 2078,
  "2021-02-15": 1794.9,
  "2021-01-15": 1339.3,
  "2020-12-15": 857.3 }
{% endhighlight %}

The JSON Plugin expects to parse data out according to a [JSON Path (JSONPath-Plus in fact)](https://github.com/JSONPath-Plus/JSONPath), where each
field has an explicit name; this data uses implicit naming. There _is_ a way around this,
but the results aren't great to look at.

Firstly, the list of "values" (in this case the meter readings) is accessible as
`$..*` ("Return every element value in the document"). That's great as far as it goes.
But I want the date. Some more ferreting around revealed [a Stack Overflow Answer](https://stackoverflow.com/questions/46471516/get-keys-in-json)
(_why isn't it in the documentation? Is it another deprecated/undocumented feature I can expect to go away sometime?_)
that `$..*~` gets the key name list. Brilliant! Except... it's auto-translated
from the human-readable date above into a Javascript timestamp (`2021-03-15` becomes `1615766400000`, the timestamp in milliseconds).
Obviously this isn't a good look in general - the variable selector becomes a list
of javascript timestamp values but at least it's still in sort order.

## Javascript Timestamps Aren't Granular Enough For Influx
In the absence of any client-side ability to turn those values back into anything
human-readable, I can however spit them straight into the Influx queries. Where
the next challenge is that they're _not precise enough_ - [Native Influx timestamps](https://www.influxdata.com/blog/tldr-tech-tips-flux-timestamps/)
(which is what a raw number like this will be interpreted as) are in Nanoseconds,
or 1000000 times the javascript value.

This means all my queries need multiplying by at least this factor, a string of
queries written like:
`SELECT reading from meterreadings where time >= $selectedBillingPeriod*1000000 limit 1`
ensues...

## Issues: Refresh.
A quick look at the server logs shows that once I'd configured the data source,
that was the last time Grafana itself actually queried the data. It doesn't
appear to matter what I set "cache" times to, _that data is not refreshed from the
json source_.

Even weirder, if I deliberately mis-configure the data source (e.g. forbidden
  query parameters, bogus URL etc) then you'll see a request from Grafana to the
source (getting a `404 Not Found`) as you'd expect. But change it back to the
working value and.... nothing, Grafana doesn't even make a request for presence.

As far as I can see this is a bug, possibly in Grafana itself but most likely
the plugin. I've subscribed/commented on [Issue #93](https://github.com/marcusolsson/grafana-json-datasource/issues/93)
to see what happens.

## Issues: Timezones.
A secondary issue: because the source data stores the dates as just dates,
with no timezone data associated, there's an off-by-one-hour issue between
"meter readings" and the raw data, with meter readings being an hour ahead in the
summer. This breaks the "Est. Current Reading" calculation (because what should
  be two time-aligned results turns into two time-out-by-an-hour readings which
  the transform doesn't like).

It turns out this is the "fault"(?) of the JSON API plugin, which is obviously
passing the simple date strings through to javascript's default parser which is
turning them into timezone-aware timestamps. There appears to be a bug in the plugin
which means overriding the type detection (I'd like to hard-code it as a "String")
doesn't work. I've raised [Issue #94](https://github.com/marcusolsson/grafana-json-datasource/issues/94)
on the plugin to see if this helps or not.

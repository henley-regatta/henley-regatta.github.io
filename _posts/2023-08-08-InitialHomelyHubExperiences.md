--- 
layout: post
title: "Initial experiences of the Homely Smart Controller in a brand-new Air Sourced Heat Pump installation"
date:  2023-08-08 08:30:00 +0100
categories: review
tags: winge heatpump homely 
--- 
_I'm writing this up, early, with limited experience because I went a-looking on
t'internets and there's very little third-party information or reviews of the
Homely system. So, why not add some mud to that water with my own thoughts?_

Apologies, this is a long long wall of text with no pictures. And there's no real conclusion to speak of either

**_update 1:  2023-08-09 13:00_**: Homely called back after leaving a message on
support line; they have fixed the account login page and I've been able to
select a Tariff that more closely matches our reality. Smart+ will get 2
nights where it **knows** there's cheaper electricity between 02:00 and 05:00 to
_actually_ run the hot water so we can have warm showers before I give in and
fall back to Smart mode...

**tl;dr:** 10/10 idea. So far, 4/10 execution. I _hope_ I will be able to
update, amend and possibly even re-write all of what follows in the near future.

The [Homely Smart Controller](https://www.homelyenergy.com/) is working - in the
sense that it switches things on and off, it's connected to the internet, it's
receiving input from it's temperature sensors, and the App works on the Phone
(the only way of controlling it).

But...

#### "Implementation" issues I'm frustrated I can't resolve: 
  - ~~I selected the wrong tariff on setup - in probably the wrong operation mode
    for our needs - and I _think_ this is why the hot water's not doing what I
    expect.~~ System on probation now after selecting correct tariff.
  - ~~I'm unable to log in to my account to rectify this and, so far, the
  support's non-existent.~~ Used the telephone option, left a message, got a
  call back within 2 hours. Issue resolved their side, can now log in to account
  and change settings.
  - There is no way to tell what temperature the Hot Water tank is currently **at**.
  - The Temperature Node is a little "black box" and you need to continuously
    monitor the App to tell if/when it's connected and working.
  - Reconnecting the Node to the Hub is a _massive pain in the arse_ especially
    if you need to go into the attic to do so (if our insulation's effective
    it'll be freezing up there come winter...)

#### "Conceptual" issues that I've got to deal with:
  - This system is making decisions about when/how to control our heating &
    hot water and it's _not explaining_ those decisions in any way. 
  - The system is 100% dependent on our trusting it. Without in any way providing
    the ability to prove that the system is, in fact, worthy of trust.
  - This is a bit more important than "My TiVo skipped an episode of my
    favourite show". So far it's limited to less-warm showers in the morning,
    but in a few months it could be waking up to and living in a cold house
    through no fault of the heat pump system but just because we don't understand
    and can't effectively manipulate the control system.

---
**_2023-09-08_**: I'm only going to update "above this line". Note that I'm leaving everything else below but some of what follows below will be out of date/obsolete and _not marked as such_. Everything **above** this line should be accurate as of your reading.


## Context

Absolute bleeding-heart liberals that we are, we've spent the last year or so
undergoing the appropriate review/assessment/preparation to move to a Heat
Pump-based system ~~that involves spending as much money as
possible to get as many toys as we can into our house~~ er to invest in a
future-friendly cost-effective system. 

Pretty much at the last minute before installation started, our installer
offered us a smarter controller solution - the [Homely Smart
Controller](https://www.homelyenergy.com/) on the grounds that it's better at
optimising use of the pump for efficiency and allows for better remote control.
I was sold on the "it's smart" aspect so enthusiastically embraced this option.

## What problem does Homely (attempt to) solve?

Heat Pump based systems are fundamentally different from traditional
boiler-based heating systems in that they're designed to run for longer at lower
temperatures to achieve their efficiency gains. From a control perspective this
means they're a lot more complicated: We're all used to running the heating
boiler a few hours a day to quickly heat the house and/or get a tank of hot
water. With a Heat Pump the same approach will mean a cold house & tank and/or
expensive electricity bills as the Pump is run at non-efficient temperatures.
Instead you're supposed to run the systems for much longer - up to 24/7 - at
much lower temperature differentials to get appropriate and cosy levels of
heating & hot water as efficiently as possible. 

30 seconds on the internet will lead you down any number of rabbit-holes of
people talking about flow temperatures, compensation curves, setback and
optimising COP. I'm only passingly interested in this stuff, so having to setup
an (effectively) hard-wired pump controller with the knowledge that getting it
wrong will mean either a cold house or a massive electricity bill _or both_
**and** that we'd need to adjust it for the changing seasons regardless of
whether our comfort demands have changed is not what I'd call My Primary
Interest. So in this sense I'm an ideal candidate for the Homely approach - a
smart box on top of a heat pump system that promises to optimise all of the
above to make sure we get our target heating/hot water outputs with the minimum of
inputs (electricity, human attention).

## How does Homely do it?

You should really go onto their website and look because to be honest that's as
much factual detail as you're going to get. In terms of what I can see: 

  - There is a Homely Hub in the attic. It's connected to the internet, and the
  Heat Pump's own controller console.
  - We have a little (Zigbee connected, battery powered) temperature "node"
    (currently) in the living room that reports actual room temperature to the Hub
  - There is a temperature probe about 1/2 way up our hot water tank that's
    linked by wire back to the Hub
  - The Hub overrides whatever's programmed in to the Heat Pump controller to
    directly control and adjust the running parameters of the Heat Pump
    system
  - There's (inevitably) an App that lets you define target schedules, set
    overrides and get (rudimentary) operational information for both heating and
    water. There's no control interface on the Hub itself, the App is the only
    way to interact with the system
  - The Hub communicates to the Homely cloud (or "AWS" as nerds know it)
    to develop a "run plan" based on a number of factors including desired
    heat/water targets, current and predicted weather conditions and expected
    electricity costs. This is downloaded to the Hub daily which then executes
    the plan to modulate the heat pump system to achieve the goals.

### That's the end of the Factual part of this presentation.
Here endeth the "Objective" part of the screed. What follows is more me getting
things off my chest than actual science:

## So what happened?

We had a 4-day retrofit exercise last week during which our entire central
heating system was ripped apart and replaced. New radiators (we need bigger ones
for the lower temperatures), lots of new pipe-work, out with the old
boiler & control system, in with the new Heat Pump and more tanks than a Panzer
division (water tank, expansion tanks, buffer tanks oh my). There's a beautiful
wall-mounted installation of pipework in the attic that interfaces the new
Heat Pump plumbing with the traditional heating loop and hot water loop. Sat
proudly amongst this is the Homely Hub box. In our upstairs cupboard is the new
300L water tank, alongside which is the Midea heat pump controller box. Two
wires go from this location to the Hub in the attic - one for the tank
temperature sensor, and the "zombie" control wire from the controller to the
Hub. 

Friday lunchtime - 3.5 days in and coming to the end - our installers start
commissioning the controller system. First I need to supply WiFi credentials to
get the hub connected to the Internet; easy. Then I'm asked to download the
Homely app to my phone and create an account. OK. So right off the bat it's new
user / password and since this an Internet-facing service I need a secure
password so I'm straight into the whole Password Manager create/review/activate
process and that's hampered a bit by the in-app sign up process timing out and
dropping me back to the start screen 3-4 times. 

Once I've created an account and _before_ we can do anything else - with the
installer guy looking over my shoulder because we've still got to pair the
temperature node - I'm forced to select a "Mode" for the account with all the
usual online "nudge" patterns leading me towards their premium "Smart+" option.
Which, after a quick check, I select (hey, I don't mind paying for a service if
it really does save costs on the heating bills). I'm then immediately told to
select a smart electricity tariff, so I hit the one that sounds right - correct
supplier but (as it turns out) wrong tariff - thinking I can go back later and
adjust it. Couple more screens of setup to define default heating/water profiles
and bingo-bango-bongo, we're in the app.

Now we can try to pair the Temp node. We follow the process but, hey ho, no
temperature reported. I leave my phone with the Installer who disappears up into
the attic to prod the Hub. Twenty minutes or so later down he comes, temperature
reporting on my screen and heating profiles suitably mucked-around-with to try
and force the system to come on **now** so we can test it. This turns out to be
harder than we think...

The Homely system is _so smart_ it knows we can all tolerate a bit of flex in our
heating. So on a fairly warm day - internal temps of 23C - we've had to :

  1. Tell Homely we'd like the house raised to 25C (the max) for an hour or two
  2. Tell Homely _not_ to apply it's 3-4C flex setting and (eventually) 
  3. Put the temperature node on a cold drinks can straight from the fridge to
  fool it into thinking the house is cooler than it actually is. 

...in order to get the system to say "AHA! HEATING REQUIRED! ACTIVATE THE HEAT
PUMP!". But, yay, it all works. Tick in the box for the installers, big "well
done" and time to tidy up and GTFO to the pub on a Friday afternoon. Success!

## OK so it works then? What's the problem?

We were advised that to properly commission the heating system - it's been
primed with glycol, water and corrosion inhibitors that all needs mixing up - we
should ensure it runs for an hour a day for a week or so. Bit of a pain, given
it's August, but OK fair enough. And, you'd think, easy enough to schedule: We
put a 1 hour block of time in to the Homely app where we want the temperature to
be the max - 25C - instead of our comfortable default. And we put it in to the
middle of the day so we'd be awake to monitor it. 

But this is a smart system. We're not actually saying "please run the system
during this time period UNTIL this temperature is reached". What we're
_actually_ saying is "We want the house to be at this temperature during this
time period". The smart system then tries to work out how to achieve this. Given
the long, long latency of a heat pump system this actually meant that to get to
25C by lunchtime on Sunday it needed to run _all night_ on Saturday night. And
it still only got to 23C in the measured room. 

Our first attempt to "spoof" the system to run therefore was to leave the target
temperature at our default - 18C because we're tight - and instead use the same
trick we pulled during commissioning. Placing the temperature node onto a
freezer pack works nicely - the Hub thinks the house has suddenly dropped to 3C
and thinks that a bit of heating might solve that. Except here comes our first
implementation issue: At some point in this process we moved the Node out of
range of the Hub. We found that out 90 minutes later by checking the App to a
nice "uh-oh" message telling us the system would be in degraded mode attempting
to hit a target based on what it knows about our heat-loss and expected weather
(and more obviously by the lack of any internal reported temperature). Hmm.

## The Temperature Node. All the fun of Wireless Connectivity, None of the Feedback of WiFi's "Wrong Password" prompts

OK so we follow the guide in the app to re-connect. Step one push little button
3 times to wake it up on Node,OK,  step 2 push button on Hub. Which is in the
attic. OK, down with the ladder up we go... Oh boy that's frustrating.
Essentially we're trying to re-pair the components but this process is
_fragile_. Both sides have to be in "pairing" mode then there's a limited window
in which the process wil occur and the only feedback you get is some
_blinkenlicht_ on both the Node and the Hub. It's not helped by the App being
quite patronising - "did you see 3 flashes or more?" - without explaining **what
you SHOULD BE looking for**. It took a good 3-4 goes which, crouched down in an
attic waiting for pairing mode to expire on the Hub before we can try again was
_very tedious indeed_. 

But we got it re-paired. Hooray. For future reference - since it appears to have
happened once since - IF it's a temporary loss of communication you can skip the
re-pair process by just pressing the Node's button **once**, observing a
long-blink and then (if it's reconnected) 3 quick flashes. It's best to move
close to the Hub before doing so for better chance of success, you can then put
it back where you found it afterwards. In normal use it appears to update the
Hub once every 15-30 minutes or so so any in-app reading newer than about an
hour, _don't mess with the node_. 

## The App. Part One - Account lack-of-management

On to the next issue: I read the guide again and find that decisions on when to
run the Heat Pump especially for hot water depend not a little on the expected
Energy Tariff. You'll recall I mis-set that to one that's "Agile" meaning it
scales up/down at short notice to exploit generating conditions. Brilliant stuff
but, unfortunately, _not the tariff I'm on_. We've recently moved to a
Time-of-Use tariff with a similar-sounding name - we pay different amounts in
peak / off-peak but (crucially) those periods are always the same. 

Presumably, therefore, any goals we've given the system in terms of when we'd
like hot water or a not-freezing house are then subject to whatever spot-prices
are being applied - if it's going to be cheap for an hour in the middle of the
day then yatzee run the system, similarly if it's expensive overnight then no
hot water for you (_this is inferred behaviour based on a couple of day's
observation **without** access to any of the cost data_)

So this is something I'd very much like to change. In the App it's under "More"
-> "Update Account Details" but this just launches to a Chrome page where
(_despite coming from the already-signed-in App_) I'm presented with a login
page. And here's where the fun starts: Every attempt to log in fails. But I'm a
nerd you can't keep me down, I find the URL  of this page and try it from my
browser. And get the same error. And then I try "Incognito" to rule out any
plugin nonsense. And then Firefox. And then Firefox Anonymous Window. All the
same.

Opening "Developer Tools" and tracing network soon reveals the problem: Hitting
the "Login" button sends a request to... an undefined AWS endpoint. Literally a
server address that's **ERR_NAME_NOT_RESOLVED** by DNS. And "By DNS" I mean my
ISP, Google and Cloudflare. So: it's not on the internet in any meaningful way.

Just to prove it's not a me-problem, we get Dearly Beloved to download and
activate her Homely App on the phone using the same user/password I setup way
back in part 2 above. And that works fine. And yet she can't get to Account
settings either.

Time for Some Support.

## The Homely Web Page and Support - Great for connecting to internet

So off we schlepp to [Homely Energy](https://homelyenergy.com/). I'm mostly not
here to complain - clearly this is first and foremost a "sales site" but they've
got quite the extensive FAQ and self-help guide. And if you're having the sort
of trouble we've been having getting your Node connected, or your Hub to the
internet, _this is the place to be_. That bouncing Homely Support Bot in the
bottom-right of the page has _got you covered_ for that. 

If, on the other hand, you've got some other problem then... off you go, digging
around looking for e.g. a ticket generation. But there ain't one. It's a phone
number or an email. So I write them a nice email explaining I can't login to my
account and wondering if this might be a me-problem or a them-problem and, oh by
the way I see there's an name-not-resolved to an AWS domain when I trace it so
perhaps it's not me?. 

That was on Friday. It's now Tuesday. No response. And the account page still
isn't working. I'm going to have to phone them, aren't I?....

## And what about the system, it's working right?

This would be fairly low-priority if the system was _working as expected_. 

I'm told, by Homely's lovely literature, that the system will optimise when and
how long it runs for but _always_ with the goal of having your schedules met. At
the moment this is simple: I want a tank of hot water at 07:00 so the family can
take a shower. I'd _like_ there to be hot water at 19:00 so we can do the
washing up and/or those of us that didn't wash in the morning can scrub before
bed. 

What I _expect_ to happen is that at some point overnight the system looks to
see what the temperature in the tank is currently, what the target temperature
is (50C as set in the app), and then come on early enough to ensure that
hot-tank-at-07:00 condition is met. So far that hasn't happened once. In 4
nights. 

First night gets a "pass": we'd only had an immersion heater the night before
(system still being commissioned) and, at the time, a dumb one at that. It ran
all night and brought the whole tank to 60C (at approximately the same energy
cost as driving my car 50 miles, in case you're wondering why immersion heaters
aren't anyone's preferred hot water solution...). And there was plenty of
hot-enough water left over on Saturday morning to meet that need.

However, Number One Son took the world's longest shower and as a consequence
there was chuff-all hot water come Saturday lunchtime. I rather hoped the system
would see this and try and meet the evening temperature goal but... nothing
happened. 

How do I know the system didn't run when we were out of the house? I wasn't
watching the app or the heat pump so _how do I know_. Well there's no history in
the app but I've got power monitoring so I can _tell_ when that 10kW heat pump
is running  (even when it's only consuming 1-2kW and not the full amount).

We manually scheduled it so we had _some_ hot water - or warm-ish water - ready
for Sunday but, again, the heat pump didn't come on overnight. And it didn't on
Sunday night either. It _did_ run for an hour or so during the day Monday to
heat hot water but, again, not overnight. 

Each morning we've had a tank of tepid-to-warm hot water. And I don't know if
this is working as designed or there's a problem. Because although we set a
target temperature in the Homely App - 50C - **nowhere** in the app does it tell
you what the _actual_ temperature is. And it _does_ know this - as I mentioned
the Hub's got it's own hot water tank sensor, it's right in the middle of the
tank and I can only presume it's working. The only actual temperature readings
we've got are one on the newly-ensmartened immersion heater (but that's at the
bottom of the tank next to cold water input so it's _expected_ to be low), and
whatever the heat pump controller's telling us. And I sincerely doubt that's
actual tank temperature because there's no other sensor in the tank; I'm
guessing it's measuring heat-exchanger return temperature which is an analog
useful for when to turn the system on/off but unreliable for actual temperature. 

## The App. Part Two: "Trust us, Bro"

This brings me to the _structural_ issue with Homely. This is a system that is
designed to hide all the complexity of a heat pump system - all these flow
temperatures, compensation curves, latent heat capacity of the structures and
all that stuff - and present a simple, easily understood user interface that
"just works". 

If it _did_ Just Work then that would be one thing. If I'd woken up every
morning to tank of water at the expected 50C, I'd have some confidence that when
the cold weather comes I would _also_ be waking up to a house at the target 18C
temperature. But that hasn't happened once yet, in a sample size of 4.

The mantra of "trust us" is endemic in Homely. Go look for a detailed
explanation of how it works on their website - all you'll find is assurances
that it's doing it's best to meet your goals as cheaply as it can. No mention of
_how_, at least not in detail. I did find an explanation, from a 3rd party, on
the internet that makes sense but honestly I've got no idea whether it's _real_
or not. So I'm not going to repeat or link to it here.

This attitude extends to the App. When I've gone into it to find the heat pump
running - either heating or water - all you get is a toast at the bottom of the
screen saying "why is my heat pump running?" which, when expanded, gives some
platitudes about "we're trying to meet your target schedules by exploiting cheap
times of day". 

In the first place it'd just help if the system told you _which_ of the targets
it was currently trying to meet. And if it's skipping one (like a full tank of
hot water in the morning), at least some explanation of why would be useful
("electricity prices are currently too high" perhaps. Or "measured tank
temperature is within the flex range" maybe). 

It's not like the Hub doesn't know this - I can see it's communicating with the
mothership overnight, presumably to generate and load the "game plan" for the
day based on whatever weather/electricity/target settings the cloud has decided
are appropriate. Some way of exposing that plan - just a log page under
"Advanced" in the app would do me - would do a hell of a lot to build my
confidence in the system and start to earn that trust the system relies on. I
get why all that detail isn't front-and-centre, it goes against what the
system's sold as, but I _cannot_ believe that even the most techno-phobic user
wouldn't find _some_ explanation of why it's doing what it's doing useful.
Especially when they wake up to a cold house to take a cold shower.

## ...But what if I don't trust you, at least not yet?

There's an argument that nearly all of the above is on me. I clicked through on
defaults when setting up the account and got put on a profile that's not
appropriate - "Smart+". And then, to compound the issue, I selected the wrong
tariff so what the system's trying to optimise for doesn't actually apply to me.
There's some truth in that but, then, don't we _all_ just click through on
defaults these days?

Certainly, at this point, **if I could get through to the account settings
page** I would strongly consider moving back to "Smart" mode in which
considerations of electric tariffs etc are discarded and the system just works
to hit the target temperatures at the defined times. I might even revert to
"Standard" mode which just energises the system during the defined schedule
periods, targets be damned, as I think that might be useful to get some
practice/confidence in the actual heat pump environment (as opposed to the heat
pump _control system_). 

But I can't. Because the only way to control this is to use the accounts page.
Which isn't working. And which isn't responding to support requests. 

Or I could climb into the attic and use the vulcan-nerve-pinch method and just
disable the whole shooting match I suppose...
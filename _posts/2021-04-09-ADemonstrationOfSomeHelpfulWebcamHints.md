---
layout: post
title:  "HOWTO: Work-around WebCam Autofocus on Windows"
date:   2021-04-09 14:38 +0100
categories: howto
tags: howto testpost webcam
---
# Working around Windows WebCam Autofocus Behaviour.
In common with _lots_ of people recently I've been using a WebCam I got for
 (fairly) cheap online for work and social purposes. It's reasonably well-specified
 for what it cost - dual mic, 1080p output, autofocus. And it's properly
 plug-and-play too - uses the default driver (at least on Windows 10) so there's
 no aggro with installation or preparation.

 There is, however, somewhat of a glitch. On (recent?) Windows 10 builds, the
 Autofocus functionality only seems to work on the "first use" of the camera
 after bootup. If you close the camera app or Discord or Zoom or whatever was
 using it, and then launch either the same app again or even a different App,
 Autofocus just... doesn't. So quite often I come back in blurry.

 The simplest fix for this is to unplug the camera and plug it back in again.
 However, if you'd like a more nerdy fix it's worth noting that manually overriding
 the focus is still possible through software. I used the built-in Windows
 Camera app for this.

 Here's what it looks like when launching the webcam for the 2nd or later time:
 ![blurredimage]

 In Window's Camera app, you can manually focus but you need advanced settings
 enabled. Go to the gear icon top-left and select "Pro mode" to show the advanced
 settings:
 ![settingsdlg]

 Then, on the left hand side of the image use the middle widget (square in a circle)
 to slide the focus bar up and down until you're back in focus:
 ![focusslider]

 Voila! A perfectly focussed image is available:
 ![focussedimage]

At this point you can quit the Camera app and relaunch or restart whatever
application you were using...

[blurredimage]: /assets/webcam-camera-blurredpic.png
{:align="center" width="150px"}

[settingsdlg]: /assets/webcam-camera-settings-dialog.png
{:align="center"}

[focusslider]: /assets/webcam-camera-focusslider.png
{:align="center"}

[focussedimage]: /assets/webcam-camera-focussedpic.png
{align="center" width="150px"}

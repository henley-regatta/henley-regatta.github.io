--- 
layout: post
title:   "HOWTO: Connecting a WaveShare ePaper display to a Raspberry Pi Pico W"
date:     2022-09-16 18:00:00 +0100
categories: howto
tags: howto wiring ePaper PiPicoW
--- 
In the realms of Helpful Information On The Internet, there are many
sources that may or may not help you do fairly common interfacing tasks.
However they're not all superbly well written _or_ appropriate for your
specific use-case. In my case, I found the fact that various web-pages and
[documentation](https://www.waveshare.com/wiki/4.2inch_e-Paper_Module) was
either:

  1. Referring to older hardware (Raspberry Pi Pico not Pico W)
  2. Not _at all_ clear whether pin numbers were physical or logical (GPIO)

...dangerously confusing. Here is my attempt to rectify that.

### Disclaimer
I've only tested this on the following specific hardware:
  * [Raspberry Pi Pico W (2022; FCC ID 2ABCB-PicoW)](https://www.raspberrypi.com/news/raspberry-pi-pico-w-your-6-iot-platform/)
  * [WaveShare 4.2inch e-Paper Module (400x300) Rev 2.1](https://www.waveshare.com/4.2inch-e-paper-module.htm)

..Although the wiring diagrams should be good for the earlier Pi Pico and any size of the WaveShare e-Paper module _provided_ it's one of the ones with the embedded controller and not the "raw" screens.

## TL;DR how to map the pins
![Table of Pin Mappings WaveShare to Various Raspberry Pi Devices](/assets/WaveShare_to_PiPico_PinLayout.png)
{:style="display: block; margin: auto; width: 90%;"}

## Discussion

Most of the online references, and indeed the sample code available for
the ePaper devices, _implicitly_ describe the logical pin numbers - the
GPIO number - which actually is printed on the Pi Pico W underneath but
_does not_ directly correspond to the physical pin location. 

The [Pi Pico Pinout
diagram](https://www.raspberrypi.com/documentation/microcontrollers/raspberry-pi-pico.html#pinout-and-design-files15)
is required to actually understand this, but even then it can get
confusing to work out what "mode" the pins are being used in. My
understanding is that the core SPI pins - `DIN/MOSI` and `CLK/SCLK` - are
using the SPI1 "mode" of physical pins 15 and 14 as `SPI1_TX` and
`SPI1_SCK` respectively. Whereas the `CS, DC, RST` and `BUSY` pins are
just being "mapped" onto available GPIO pins on the PiPico/W. This is
confusing since those pins are _also_ used for SPI according to the
documentation. However, the fact that each of these pins is defined in the
sample ePaper code Python files with reference to the GPIO number and not
the physical pin or SPI value supports this belief.

It is also lucky for us (I think) that the sample code uses the SPI1 bus,
since the Pi Pico W uses SPI0 for the onboard WiFi interface; I'm not sure
this would be a deal breaker but it might be a complication worth
avoiding.

## But does it work?
### Yes.
![Picture showing PiPicoW upside down with coloured wiring going from pins listed in table to a 4.2 Inch e-Paper module, itself showing the results of the WaveShare demo Python script onscreen](/assets/PiPicoW_ePaperModuleInterface.jpg)
{:style="display: block; margin: auto; width: 75%;"}

Although you can probably do a better job of the soldering of the header
pins than I did, it does in fact work and I got the demo file to run
(on the first attempt!) to display output. I should note that this did, in
fact, require patience because the REPL showed several iterations of
`e-Paper busy` / `e-Paper busy release` before _any_ visible change was
made to the screen, and that was a black wipe/refresh before drawing
output. But, hey, if it's millisecond refresh rate you wanted, then an
LCD/OLED display using HDMI should be your tool of choice not this
multi-second refreh ePaper stuff...

***
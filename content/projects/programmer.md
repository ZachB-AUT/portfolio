---
title:  "ENEL602 Test Device"
date: "2026-05-08"
draft: false
# mathjax: true
# category: projects
---

## Overview

I am a teaching assistant at AUT for a second / third year paper called ENEL602
(Electronics Project.) As part of that course, students design and build a small
line-following robot.

> Insert image of robot here.

One problem I noticed when I was taking the course myself was that after the
robot was complete, students were expected to test it by writing code for the
onboard microcontroller. However, if (or inevitably *when*) they encountered
some technical issue, how were they supposed to know if the problem is with
their physical assembly, or with their code?

To solve this, I threw together a Rube-Goldberg machine consisting of a
Raspberry Pi 3b, a USB-ASP programmer, 3 buttons, and a python script.

The idea is that when you connect the robot to the programmer and press a
button, it then writes a known-good test program to the microcontroller, and if
it works the students can know they assembled the robot correctly.

## How It Works

The sequence of events this device relies on is relatively simply. A Systemd service starts a python script which watches a GPIO pin. When that pin is grounded the script calls a `subprocess.run()` command which uses AVRDUDE to flash a precompiled binary to the connected microcontroller.

It was worse before, the first iteration used the same makefile I wrote when I was taking the course, so the chain was systemd service -> python script -> makefile -> AVRDUDE.

## Outcomes

While I originally made this device for the benefit of the students (and it really did seem to help!), it turns out it was also extremely useful for the lecturer who hired me.

In order to mark student robots, the lecturer had previously been taking all of them to the microcontroller lab room down the hall, flashing them using the computers in that room, and then bringing them back to the electrical lab room to test.

By making a device that can very quickly flash them *in the same room where they are tested*, I was able to save the lecturer something like 10 hours of grading time.

I am convinced this played a significant role in why I was rehired the subsequent semester.

## Next Steps

I plan on working on a better version of this. Currently the test device is way overkill for this project. In the future I plan on making a standalone device to handle this usecase, likely something similar to Thomas Fischl's [ISPnub](https://www.fischl.de/ispnub/).

Almost all of the processing power of the atmega8 on the USB-ASP programmer (also designed by Thomas Fischl!) goes toward running the USB driver. Almost any microcontroller is capable of transmitting over ISP. I plan on making something very low power which is capable of transmitting the test program.

## Afterthought: Being Stubborn and Annoying

Part of the reason this project was relatively easy for me is because I already
figured out how to flash AVR microcontrollers from Linux/MacOS. The expected workflow from the university requires students to use ATMEL Microchip Studio, which is A: garbage and B: only works on windows (which I don't have). Therefore, I spent a bit of time figuring out how to compile / flash code onto the microcontrollers from my laptop.

While I wouldn't go so far as to say this is a *good* trait on my part, so far being stubborn and annoying about using tools I like has only ever paid off for me.




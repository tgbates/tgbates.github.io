---
layout: post
title: Setting up a MIDI Keyboard in Linux Mint 17.1
date: 2016-01-11 08:00:00 -0600
category: linux
tags: [linux, mint, music]
---

# Setting up a MIDI Keyboard

I bought a MIDI Keyboard last year and this was the process to get it working.  
[Rafal Cieslak\'s article](https://rafalcieslak.wordpress.com/2012/08/29/usb-midi-controllers-and-making-music-with-ubuntu/) was helpful in getting me running and understanding what I was doing.  I used another article for the setup but it is no longer online.


## Synaptic
install jack, jackd, qsynth and qjackctl

## Qsynth
click Setup -> Soundfonts -> Open and add the sf2 file in /usr/share/sounds/sf2

In my install, it was TimGM6mb.sf2

Click 'Yes' to restart the engine

![qsynth setup](/assets/qsynth_setup01.jpg "qsynth setup")

## Plug in your USB midi controller

## Open Qjackctl

click Connect

Select the ALSA tab, drag your midi controller to FluidSynth

![qjackctl](/assets/qjackctl_01.jpg "qjackctl")

## Play Music!
 

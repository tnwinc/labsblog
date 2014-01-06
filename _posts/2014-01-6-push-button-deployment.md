---
title: "Push Button Deployment - Literally"
layout: post
tags: [hardware, launchpad, arduino, 3D printing, teamcity, deploy]
published: true
author: Mark Crow
mail: "MarkCrow@tnwinc.com"
summary: A simple, yet overkill project utilizing various rapid prototyping systems to create a fun USB device.
---

We are constantly tweaking our code deployment process and the buzz word that kept coming up in this phase was "push button" deploys to any environment. Manual processes were replaced by scripts and more control was pulled into TeamCity. While the changes were being developed, the team joked about literally delivering the upgraded solution with a physical push button device to initiate the deploy. That sounded like fun.

The first thing that came to mind was having two key switches, an "armed" indicator and the obligatory big red button. Think of the James Bond Golden-Eye scenes or a missile launch command station. I've seen USB devices like this for sale, but making a customized one is more my style. The first step was to create a cardboard mock-up to get the creative juices flowing. An [Arduino](http://arduino.cc/) simply supplied the power to the circuit from the USB host.

![mock-up](/screenshots/launch_box/mock-up.jpg "First cardboard mock-up, without any real hardware.")

Next, we need to get the electronic components, wire it up and write some code. Since this was just for show, I procured only the finest components (to be had for less than $10). I'm very familiar with using the Arduino platform to control devices, but an Uno is a $30 device. James graciously donated a cheaper Texas Instruments LaunchPad to use instead. I hadn't worked with a LaunchPad before and I knew it used a different IDE than an Arduino. However, I found the excellent [Energia platform](http://energia.nu/) which lets you code for the LaunchPad just like an Arduino. The switches and button were delivered and I verified the sketch still worked.

![electronics](/screenshots/launch_box/electronics_draft.jpg "Arduino and LaunchPad working on the same code. Nice.")

The code very simply checks the status of each key switch, sends power to the armed LED, then sends a serial command via USB when the button is pressed. On the receiving end, I wrote a C# app that detects the device being connected and waits to get the deploy command. Once the command is received, it passes it to TeamCity's API. The app is configurable to specify what branch and destination will be used. Now that the device was functional, it was time to replace the mock-up with a prototype. Below are the electronic guts and updated enclosure.

![prototype](/screenshots/launch_box/prototype.jpg "Prototype design.")

Cardboard and hot glue makes a decent placeholder, but we wanted something a bit more durable. The enclosure could be brushed metal via CNC mill, dark shade Plexiglas via CNC laser cutter or 3D printed in ABS plastic. I went with 3D printing because I hadn't tried something that large on my printer. My currently preferred tool for designing primitive based 3D models is SketchUp. In very little time the enclosure was ready to print. 

![sketchup](/screenshots/launch_box/sketchup_model.jpg "Sketchup model.")

Unfortunately, I ran into several issues with the print because of ABS's dimensional instability. I had edges curl and layers separate, but the end result was acceptable. The silver lining was these issues triggered several upgrade projects to my printer. Adding a magnetic Plexiglas enclosure helped with layer separation and a heated glass platform coated with AquaNet / ABS slurry helped with edge curling. The layer gaps could be filled with ABS "putty" by adding more plastic to the ABS and acetone slurry. The enclosure will eventually be artificially distressed to give it a more industrial look.

Now that the electronics had a place to live, it was time to ditch the breadboard for a soldered PCB. I didn't want to etch the PCB, so I instead left the breadboard headers in place and ran bus style connections. Normally you wouldn't design the circuit like this, but this wasn't serious business. 

![wiring](/screenshots/launch_box/wiring.jpg "Device wiring.")

We turned the keys, hit the button and off went the deploy to a QA environment. The device can easily be reconfigured or repurposed as needed.

![device](/screenshots/launch_box/final.jpg "Device hooked up to workstation.")

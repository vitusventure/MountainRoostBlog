---

layout: post
title: Beetle Flasher Replacement
typora-root-url: ..
---

I recently purchased a 1968 VW Beetle from a neighbor down the road. I have never really been a "car guy", but I do like working on things, and this seemed like a fun project. While parts of it have been more "fun" than anticipated (so much rust...), I have gotten to actually accomplish a few things. I wanted to get the car drivable ASAP so I can actually enjoy it a bit rather than just work on it. Luckily, it runs quite well and only has a few holes in the floor. The main thing I needed to fix was the signals...

## The flashers that didn't

When using the turn signal switch either side would blink just once or twice then stop. Attempting to engage the hazards did nothing. After some research, I determined that the infamous 9 pin flasher box had gone bad. Replacements are available, but they are expensive (relatively). I wanted a fix using off the shelf parts in a way that would be easy to fix again in the future. 

As a note, this is far from stock. The Beetle is already well on its way to a Baja so I was not really interested in a restoration.

## Off the shelf parts

After reasoning about the function of the flasher system a bit, I designed a drop in replacement that uses commonly available parts. The only thing that is slightly weird are a few diodes, but even those are the super common 1N4001 type that I had sitting around. A bit of soldering is involved to install the diodes inline, but the project is otherwise just crimped on connectors.

The needed parts:

* EP-35 Flasher module (from auto parts store)
* DPDT 12V relay (ordered from Amazon because no local stores had it)
* 2 1N4001 diodes

### Why diodes?

The diodes are used to isolate the left side and right side flashers from each other when engaging the hazard circuit. If the diodes were not in place, the normal left/right signals would trigger all four signals by back feeding at the hazard connection.

### The replacement

<img src="/assets/images/beetle/flasher_diagram.jpg" alt="flasher_diagram" style="zoom:50%;" />

Here is the diagram for the replacement. By default, the flasher is powered from the ignition source and connected to the turn signal switch. However, when the relay is activated via the hazard switch, the flasher is connected to unswitched power and to both sets of signals (with the diodes for isolation).

Here it is lovingly installed (with zipties):

<img src="/assets/images/beetle/flasher_installed.jpg" alt="flasher_installed" style="zoom:50%;" />

And it works! A relatively quick build with common parts that can be sourced most anywhere.

## The drawback

The only drawback for this arrangement is with the turn signal indicator in the dash. For most cars, voltage is supplied when the light should be on. But for the old Beetle, the flasher is expected to ground the circuit. The net result here is that the dash indicator flashes opposite the signals (when the signals are engaged, the dash light is on when the signals are dark). However, it still works and only flashes when the signals are engaged (either via the turn signal switch or hazard switch).

A reasonable trade off (in my mind) for a cheap replacement with commonly available parts!
---
layout: post
title: Building a Smart Home
---

# Building a smart home

For years, I have generally been of the opinion that the "Internet of Things" is mostly a way to introduce a lot of 
vulnerabilities into a home network for dubious gain. Especially since most smart home systems I had seen involved a number
of manufacturer specific apps that didn't really seem to communicate together. Why spend extra to have 20 more apps on my 
phone that only worked when the internet did?

Recently however, I have been reinvestigating the smart home landscape, and I realized that I missed a large contingent of 
home automation solutions that work locally, are open source, and make for a fun project. I just recently bought my first
home, so I am finally in the position to upgrade my living space without just contributing to my landlords bottom line.

## Starting small

The first upgrade I did was to install a Lutron Caseta dimmer in my (2 year old) son's bedroom. This was originally done because
we needed a way to turn the lights off after reading but before bed, without getting up and disturbing the boy. The Caseta
dimmer worked great for this purpose (using the smart remote). Initially, I planned to eventually add Caseta dimmers all 
through the house, along with Lutron's bridge.

However, as I started to learn more about the current home automation landscape, my plan changed. While the Caseta system is
highly regarded around the internet, it was really a closed system. The switches use a proprietary communication protocol and
really only worked with Lutron's Smart Bridge. The consumer version of the Smart Bridge doesn't offer local control, so if the
internet is out, you are back to pushing switches manually (truly a dark situation).

## Going Open Source

While you can buy Lutron's Smart Bridge Pro, which offers local control via Telnet, I wanted to investigate other, more open 
solutions. I first ran across [OpenHAB](https://www.openhab.org/), but the interface was clunky at best. I was much
more impressed by [HomeAssistant](https://www.home-assistant.io/). HomeAssistant (HA) is written in Python (my favourite),
and offered tons of integrations with different systems. I also had a Raspberry Pi 3 B+ sitting unused which is the
most common way to get HA running.

Once I settled on giving HA a try, I needed to pick the technology I planned to use around the house. There are generally two
of protocols for local control, ZWave and Zigbee. The end result of either system is similar, a local radio network of 
devices that don't connect to the internet. USB radios are available for both network types, to bridge between the local
mesh network and HA. Even better, HA can handle both, so there really isn't a requirement to choose before starting out!

After doing some reading and looking at the available devices, I decided to start with ZWave. There seemed to be more
device availability using this protocol, even though the protocol itself doesn't seem quite as nice as Zigbee (Google around 
for comparisons). The choice was relatively easy to make, especially since I can always add Zigbee later on.

## Next steps

This is really just an overview of what I plan to build. I intend to outline the various upgrades I make around the house
as I add more and more automation. I like that it all can be added piecemeal, especially since the individual bits
can be leveraged as soon as they are installed!

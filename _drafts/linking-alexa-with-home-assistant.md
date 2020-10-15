---
layout: post
title: Linking Alexa with Home Assistant
---

While Home Assistant's app is a great way to manually control automations, I wanted to go full scifi home-from-the-future and add a voice assistant to the mix. Many people rightfully have privacy concerns regarding bringing smart speakers into their homes but I decided that the convenience of a voice assistant outweighed the concerns (real or imagined). Each person should make that determination on their own! Hopefully, there will be an open source alternative that is as easy as dropping some Echo Dots around the house in the future, but the open solutions are just not there yet.

## Home Assistant Cloud vs Rolling Your Own

For the easiest integration between Home Assistant and Alexa, you can just subscribe to Home Assistant's cloud solution. The price is relatively low ($5/month at this time), and it is a "just works" solution. However, if you are like me, there is some joy in building out your own solution which is free (or very close to free).

Home Assistant [provides some instructions](https://www.home-assistant.io/integrations/alexa.smart_home/) on setting up your own integration between your local instance and Alexa, but the instructions are very manual, especially when building out the AWS resources. There are a bunch of things to create and a bunch of things to think about if you decide to tear it down. Luckily, AWS provides just the service to remove the pain from this process!

## Home Assistant -> Alexa via Cloudformation

Cloudformation is an AWS service that lets you define AWS resources in a template. You hand a template to the service and it creates all the resources across the various AWS services. The resultant entity is called a "Stack". Once you are done with the resources, simply delete the stack and everything goes away. This is much nicer than manually creating a bunch of stuff. Just upload some YAML, provide some parameters, and all the AWS setup is done!

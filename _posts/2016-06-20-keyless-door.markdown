---
title:  "Keyless Entry Front Door"
date:   2016-06-20 11:00:00 -0500
categories: door smarthome
comments: true
---
The first part of the project is to enable keyless entry for the front door. The door right now has a single-cylinder door handle, and no dead-bolt. I'm adding a KwikSet Downtown deadbolt, and replacing the door handle with a matching style, also from KwikSet so that they are keyed alike. Any existing deadbolt and door handle will do, though. this is purely a style choice.

<!--more-->
![Downtown Deadlock]({{site.baseurl}}/images/door-handle.png)

The deadbolt will be controlled by an August HomeKit enabled lock. The August lock replaces the inside portion of your deadbolt with a homekit enabled solenoid which turns the lock mechanism for you. Since its already HomeKit enabled, I'll simply be able to tell Siri to lock or unlock the front door, and can geofence it so that it opens automatically when I approach.

* <a href="http://august.com/products/august-smart-lock/">August Smart Lock</a>

![August Smart Lock]({{site.baseurl}}/images/smartlock.jpg)

The door knob is a little tricker, and will have a custom-build RFID access system so that I can enter using the tags I've implanted in my hands. This setup will use an HES8000 electronic door strike, to retract the strike mechanism allowing me to simply oush the door open. The strike will fail-secure defaulting back to the keyed entry, and access will be controlled using custom software and circuitry on an Intel Edison in the basement.

* <a href="http://www.hesinnovations.com/en/site/hesinnovations/products/electric-strikes/8000-series/">HES8000 Door Strike</a>
* <a href="https://software.intel.com/en-us/iot/hardware/edison">Intel Edison</a>
* <a href="http://www.ibtechnology.co.uk/products/quadtag-product.htm">Micro RWD Quad Tag RFID Chip</a>
* <a href="https://github.com/nfarina/homebridge">HomeBridge</a>

This setup will give full keyless entry to the house, remote control over the deadbolt incase I forgot to lock the door, and fail-secure to standard keyed entry. I'll also be writing a custom HomeBridge plugin for the electric strike so that I can geofence that as well, allowing me iOS controlled primity access to the home. just walk up, and push the door open - even if both the deadbolt and handle are locked!

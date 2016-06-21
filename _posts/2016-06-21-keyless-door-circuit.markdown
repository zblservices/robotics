---
title:  "HES8000 Electric Door Strike Wiring Diagram"
date:   2016-06-21 06:40:00 -0500
categories: door smarthome
comments: true
---
Here's a quick mockup of the electric circuit that'll control the HES8000 Electric Strike. It's a pretty simple setup with a 12V DC power source, an LM7805 DC converter to power the Intel Edison, and a 12V Relay to trigger the strike.
<!--more-->

![Circuit Diagram]({{site.baseurl}}/images/door-circuit.jpg)

Once this basic function is working, I'll add in the RFID cicuit which will actually switch the relay, giving me keyless access to the house.

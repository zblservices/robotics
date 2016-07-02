---
title:  "HomeKit Scenes and Triggers"
date:   2016-07-01 19:00 -0500
categories: xfinity automation smarthome security scenes triggers
comments: true
---
### Overview
I had my XFinity Home security system installed this week as well. The security system is a pretty straight forward setup, it's basically a bunch of wireless devices all running off their own WiFi router that's separate from my home network.

XFinity Home offers some really great APIs for custom integrations and web control. For now I've opted to use <a href="https://github.com/nfarina/homebridge">Home Bridge</a> and the corresponding <a href="https://github.com/nfarina/homebridge-icontrol">Home Bridge iControl</a> plugin to enable HomeKit integration for the XFinity Home security system.

The Home Bridge setup is running on an Intel Edison which I have mounted in my basement... Homebridge effectively turns my Edison into a HomeKit bridge, capable of arming, disarming, and providing status on my alarm system. The integration is pretty light -- I do not have HomeKit visibility to the individual sensors. However, for the purpose of home automation, that doesn't bother me so much. The important tasks are that the alarm is armed when I put the home into "night mode", or when I leave the home. Fortunately, it's great for that.

### HomeKit Scenes and Triggers
The alarm system was my first opportunity to set up scenes and triggers in HomeKit. iOS 9 does not ship any HomeKit controller applications, so I grabbed <a href="https://itunes.apple.com/us/app/home-smart-home-automation/id995994352?mt=8">Home - Smart Home Automation</a> on the App Store.

__"Hey Siri, Good Night"__
Within the Home app, I have created a scene called "Good Night". The name of the scene becomes a custom command that I can give to Siri: "Good night, Siri" (or, if I don't feel like holding down that pesky home button: "Hey Siri, good night!"). Within a scene, you can define target-states for specific attributes of any of your HomeKit devices. In this case, my alarm system has an attribute called "State", with possible values of: "Home," "Away," "Night," or "Disarm." In the Good Night scene, I set the state attribute to "Night".

My August Smart Lock has an attribute called "State" as well, with target-states of "Secured" or "Unsecured." In the same scene, I set the state attribute of my smart lock to "Secured."

Now, when I say good night to Siri, she locks my door and arms the security system.

__Arriving Home__
I also setup a trigger called "I'm Home". This trigger is tripped when I arrive within 250 feet of my home. Similar to a scene, you can set specific states for each attribute of any of your HomeKit devices. For this scene, I set the alarm state to "Disarmed" and the August Smart Lock state to "Unsecured".

Now, as long as my phone is on me -- when I arrive home after work, my alarm disarms and my deadbolt retracts, automatically.

### Conclusion
Not bad for a couple day's work. I also have several scenes defined for the Hue lights throughout the house. Not sure if that'll be worth a write up, but maybe if I have time over the weekend.

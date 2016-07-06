---
title:  "Home Network"
date:   2016-07-02 02:00 -0500
categories: automation smarthome homebridge
comments: true
---
### Overview
Since most of the devices in my home are now connected, it's really critical to understand the importance of a good wireless network. For the most part, this can be accomplished with little to no actual effort - but even those friends of mine who _should_ know better often make comments that make me think they don't.

In particular, many of my friends and colleagues have commented to me that they don't get the level of throughput they pay for from our local Cable service provider (Comcast XFinity). I have their Blast! 150 mbps service, and acheive 175 mbps downloads in almost every room of my house on the wireless network.

Here's a typical result that I just ran as I was typing this:

<a href="http://www.speedtest.net/my-result/5455636260"><img src="http://www.speedtest.net/result/5455636260.png" /></a>

So let's review the network setup...

### Cable Drop from Provider
One of the easiest, and most important things you can do, for your home internet service is actually immediately at the cable drop coming into your home from your ISP. When I bought my house, it was feeding a three-way split. Two branches fed off to the second floor of my house, and the third branch fed another two-way split for a total of four branches. One of those branches feeds the first floor, and one of those branches feeds the basement.

While there's four branches coming immediately off the cable-drop, there's actually _more_ splits in the house somewhere, because it's feeding a total of 7 in-wall COAX ports throughout the house. All of these splits lead to significant signal degredation, though, even if they're just terminal ends with no devices. When I first hooked up my cable modem, I was seeing downstream power-levels around -18 dBmV with dozens of service interruptions everyday.

If you don't know how to check this -- try navigating to http://192.168.100.1 in your browser. That'll get you to your own Cable Modem's status page for almost every popular commercially available model. Find the status or signal levels page, and check our your downstream signal level. Depending on the level of service (bandwidth) you pay for, you'll see 1, 2, 4, 8, 16, or even 32 bonded channels. The signal quality on each should be approximately the same.

In general, you want that number to be as close to 0 as possible, with an optimal range of -12 dBmV to +12 dBmV.

The solution in my home was simple: Since I have no cable service, and only need the Cable Model - I disconnected the drop from the splitters all together, and joined the drop to a RG6-Q cable which runs to my network center in the furnace room.

My signal levels on all 16 bonded channels are now between 0.0 and 2.0 dBmV. Upstream power levels are a different story, but mine are a health 38-40 dBmV on all 4 bonded upstream channels.

![Signal Power Levels]({{site.baseurl}}/images/network-levels.png)

Since most people will also want to run cable boxes and other devices, I recommend running the ISP drop directly into a two-way split. One branch of that split should feed directly to your cable modem, and the other should feed a dedicated splitter which runs to the other set-top boxes in your home.

### Wireless Network
From there, the cable modem feeds a wired gigabit router. I ran CAT-6 into my basement office and first-floor living room off that router. In each of those rooms, then, I have an 802.11AC wireless access point in bridged mode, creating a roaming network that covers the entire house.

In the basement and first floor, then, I have 600+ Mbps wireless on all clients on either access point. The first-floor access point also has great coverage on the second floor. The furthest corners of my house -- the master bathroom, and my daughter's room, both achieve 50 mbps service over the wifi.

![Wifi Power Level]({{site.baseurl}}/images/wifilevel.png)

This means that from any point in my house, multiple clients can simultaneously consume multiple HD video streams without impacting any other device on the network.

### Overkill?
Perhaps it's overkill ... but consider that at any point in time, both my living room and bedroom Apple TV's might be streaming media, and I have 5 Nest security cameras each pushing out a 1080p stream. At night when my daughter is winding down, I usually have the camera in her room streaming on my iPad so I can keep an eye on her. So it's not unreasonable to see between three and five HD stream simultaneously. Then there's the lower bandwidth devices that sit passively by, and the occassional large file transfer for work from my home office.

However, the point really is -- if you're not getting the bandwidth your paying for, do the following:

* First, connect your computer directly to the cable-modem with a CAT5e or CAT6 cable and hit http://speedtest.net. If you're seeing the speeds you expect there, but aren't getting them over your wi-fi... investigate your wi-fi capabilities and choice of router. If you're paying for over 100mbps, you really need an 802.11ac network.
* If you're testing slow with the direct connection, trace your cabling back to the ISP drop. Eliminate as many splits as possible between the drop and your cable modem.

Note - if you live in an apartment complex, a lot of this may be out of your control, however, the service provider __can__, with proper nudging, drop a second line to your unit to increase the downstream power levels seen by your devices. Don't hesitate to be pushy!

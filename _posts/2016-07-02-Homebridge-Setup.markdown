---
title:  "Getting Up and Running with HomeBridge"
date:   2016-07-02 02:00 -0500
categories: automation smarthome homebridge
comments: true
---
### Overview
Several of the devices I'm using in my Smart Home project do not have support for HomeKit, in particular:

* XFinity Home Security
* Nest Cameras
* Nest Thermostat
* MyQ Garage Doors

To tie these in, I'm using <a href="https://github.com/nfarina/homebridge">HomeBridge</a> as a HomeKit bridge to tie in automations for these unsupported devices.

### Getting Up and Running
HomeBridge is a lightweight NodeJS server that runs on your home network. I decided to install it on an <a href="https://software.intel.com/en-us/iot/hardware/edison">Intel Edison</a> which I've mounted in the basement. The edison is a low-power, very small Single Board Computer (SBC) with very simple setup and WiFi configuration. A Raspberry Pi, BeagleBone Black, or always-on Desktop would have worked equally well.

Installing HomeBridge is very quick and easy ... assuming you have npm installed, just run:

{% highlight bash %}
npm -g i homebridge
{% endhighlight %}

This will install the HomeBridge server. By itself, it's not very useful. The server is essentially a micro-kernel into which you must install plugins. I've installed homebridge-nest, homebridge-icontrol, and homebridge-myq, like so:

{% highlight bash %}
npm -g i homebridge-nest
npm -g i homebridge-myq
npm -g i homebridge-icontrol
{% endhighlight %}

### Configuring Accessories and Platforms
The first step is to create the empty configuration file.

{% highlight bash %}
sudo mkdir /etc/homebridge
cp config-sample.json /etc/homebridge/config.json
{% endhighlight %}

This empty configuration file looks like this:


{% highlight json %}
{
    "bridge": {
        "name": "Homebridge",
        "username": "CC:22:3D:E3:CE:30",
        "port": 51826,
        "pin": "031-45-154"
    },

    "description": "This is an example configuration file with one fake accessory and one fake platform. You can use this as a template for creating your own configuration file containing devices you actually own.",

    "accessories": [
        {
            "accessory": "WeMo",
            "name": "Coffee Maker"
        }
    ],

    "platforms": [
        {
            "platform" : "PhilipsHue",
            "name" : "Hue"
        }
    ]
}
{% endhighlight %}

You should now be able to start HomeBridge using the following command. Note that if you omit the -U option, homebridge will look for the config.json file in ~/.homebridge by default.

{% highlight bash %}
homebridge -U /etc/homebridge
{% endhighlight %}

### Registering with HomeKit
When HomeBridge starts up, you'll see output similar to the following:

{% highlight conf %}
[7/2/2016, 9:59:00 AM] Loaded plugin: homebridge-icontrol
[7/2/2016, 9:59:00 AM] Registering accessory 'homebridge-icontrol.iControl'
[7/2/2016, 9:59:00 AM] ---
[7/2/2016, 9:59:00 AM] Loaded plugin: homebridge-nest
[7/2/2016, 9:59:00 AM] Registering platform 'homebridge-nest.Nest'
[7/2/2016, 9:59:00 AM] ---
Scan this code with your HomeKit App on your iOS device to pair with Homebridge:

    ┌────────────┐     
    │ 031-45-154 │     
    └────────────┘     

[7/2/2016, 9:59:00 AM] Homebridge is running on port 51826.
{% endhighlight %}

Using your favorite HomeKit app on iOS, simply add a new accessory, and scan the pin code off the screen. Your homebridge server is now a bridge for HomeKit, and any accessories or platforms you add to it will show up in your HomeKit App!

Let's try it out!

### Adding Nest
Nest was by far the more complicated plugin to configure, only because it involves registering as a Nest Developer and creating a new device in your developer account. Once you have that setup, you'll have a client id, client secret, and authorization code so that homebridge can communicate to your Nest account and devices.

The instructions <a href="https://www.npmjs.com/package/homebridge-nest">can be found here</a>.

Once your setup, your config.json file should look similar to this:

{% highlight json %}
Timothys-MacBook-Pro:~ timfanelli$ cat .homebridge/config-smarthome.json
{
    "bridge": {
        "name": "Homebridge",
        "username": "CC:22:3D:E3:CE:30",
        "port": 51826,
        "pin": "031-45-154"
    },

    "description": "ZBL Smart Home",

    "accessories": [
    ],

    "platforms": [
	  {
		  "platform": "Nest",
		  "clientId": "<product id goes here>",
		  "clientSecret": "<product secret goes here>",
		  "code": "ABC12345",

		  "username" : "username",
	 	  "password" : "password"
	  }    
    ]
}
{% endhighlight %}

You'll get the product id and product secret when you create the product in your Nest Developer profile. The code is your authorization code which is obtained by navigating to the product's authorization URL and connecting it to your Nest account.

Once this is done, restart Home Bridge. In the console output, you'll see a log statement giving you a token to use. You need to add that token to the platform in your config.json, and then restart HomeBridge again. Your platforms section should now look like this:

{% highlight json %}
"platforms": [
{
  "platform": "Nest",
  "token": "<token goes here>",
  "clientId": "<product id goes here>",
  "clientSecret": "<product secret goes here>",
  "code": "ABC12345",

  "username" : "username",
  "password" : "password"
}    

{% endhighlight %}

Once you restart HomeBridge with the token, you'll see output similar to this:

{% highlight config %}
Timothys-MacBook-Pro:.homebridge timfanelli$ homebridge
[7/2/2016, 10:09:04 AM] Loaded plugin: homebridge-icontrol
[7/2/2016, 10:09:04 AM] Registering accessory 'homebridge-icontrol.iControl'
[7/2/2016, 10:09:04 AM] ---
[7/2/2016, 10:09:04 AM] Loaded plugin: homebridge-nest
[7/2/2016, 10:09:04 AM] Registering platform 'homebridge-nest.Nest'
[7/2/2016, 10:09:04 AM] ---
[7/2/2016, 10:09:04 AM] Loaded config.json with 0 accessories and 1 platforms.
[7/2/2016, 10:09:04 AM] ---
[7/2/2016, 10:09:04 AM] Loading 1 platforms...
[7/2/2016, 10:09:04 AM] Initializing Nest platform...
[7/2/2016, 10:09:04 AM] Fetching Nest devices.
[7/2/2016, 10:09:04 AM] Loading 0 accessories...
[7/2/2016, 10:09:05 AM] Software version for Living Room Thermostat is: 5.5-5
[7/2/2016, 10:09:05 AM] Temperature unit for Living Room Thermostat is: Fahrenheit
[7/2/2016, 10:09:05 AM] Current temperature for Living Room Thermostat is: 74 F
[7/2/2016, 10:09:05 AM] Current humidity for Living Room Thermostat is: 55%
[7/2/2016, 10:09:05 AM] Target temperature for Living Room Thermostat is: 69 F
[7/2/2016, 10:09:05 AM] Away for Living Room Thermostat is: 0
[7/2/2016, 10:09:05 AM] Software version for Dining Room Camera is: 205-600052
[7/2/2016, 10:09:05 AM] Away for Dining Room Camera is: 0
[7/2/2016, 10:09:05 AM] Software version for Living Room Camera is: 205-600052
[7/2/2016, 10:09:05 AM] Away for Living Room Camera is: 0
[7/2/2016, 10:09:05 AM] Software version for Kids Room Camera is: 205-600052
[7/2/2016, 10:09:05 AM] Away for Kids Room Camera is: 0
[7/2/2016, 10:09:05 AM] Software version for Office Camera is: 205-600052
[7/2/2016, 10:09:05 AM] Away for Office Camera is: 0
[7/2/2016, 10:09:05 AM] Initializing platform accessory 'Living Room Thermostat'...
[7/2/2016, 10:09:05 AM] Initializing platform accessory 'Dining Room Camera'...
[7/2/2016, 10:09:05 AM] Initializing platform accessory 'Living Room Camera'...
[7/2/2016, 10:09:05 AM] Initializing platform accessory 'Kids Room Camera'...
[7/2/2016, 10:09:05 AM] Initializing platform accessory 'Office Camera'...
Scan this code with your HomeKit App on your iOS device to pair with Homebridge:

    ┌────────────┐     
    │ 031-45-155 │     
    └────────────┘     

[7/2/2016, 10:09:05 AM] Homebridge is running on port 51826.
{% endhighlight %}

You can see in the output that it successfully registered each of my Nest devices (several cameras and a thermostat). These devices will now appear in your HomeKit application on iOS! You can name them, and then give Siri commands to control them.

### Setting Up iControl
iControl, the plugin for XFinity Home, was a much simpler setup. You simply configure it as an accessory using your XFinity Home login credentials and 4-digit alarm pin code like so:

{% highlight json %}
{
  "accessories": [
    {
      "accessory": "iControl",
      "name": "Xfinity Home",
      "system": "XFINITY_HOME",
      "email": "your@email.com",
      "password": "<your password>",
      "pin": "<your 4-digit alarm pin code>"
    }
  ]
}
{% endhighlight %}

### Conclusion
This very simple open-source offering has made quick and easy work of tying two non-HomeKit devices into my automation system. I can now control the device states and settings through HomeKit, including them in Scenes and Triggers.

One tiny quirk on the Nest integration - while Siri can turn the Thermostat off, she doesn't seem to be able to set it back to Heat using voice commands. My HomeKit scenes and triggers work just fine for that, so it's no big deal - just an odd quirk.

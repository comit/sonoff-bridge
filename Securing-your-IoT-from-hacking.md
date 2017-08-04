# General weaknesses and points of intrusion

Whenever you add devices to your network you generate additional points of potential intrusion. This is not only valid for your mobile phones and computers, but also for you Smart TV, you Alexa, or all of your SONOFF devices (ESP8266).

There are following potential risk you have to mitigate:
- Someone hacks your device and is able to login into your WLAN. (why is this a problem? 1)
- Someone hacks your device and is able to read and change any value on your MQTT server (why is this a problem? 2)
- Someone hacks your network and can interact with your devices (why is this a problem? 3)
- Someone hacks your device and use it for different things like mail bot or DOS (Denial of Service) device or WLAN jammer (why is this a problem? 4)

(1)
If someone is able to get your WLAN key, he can login into your network, if he is nearby and scan for any traffic and for any devices. Many communication is not encrypted in your WLAN by default. Therefore be part of you WLAN gives you great opportunities to screw-up the rest of your infrastructure. Also be part of your WLAN does mean, that the attacker can use your IP-Adress and your traffic to do nasty things.

(2)
If you can hack a SONOFF you might get access to the keys stored in the device. For example, the MQTT password allows you to read ALL of your devices and change any device at any time. With the information of the MQTT-Server user/password, it might be not required anymore to physically be in your WLAN. Maybe your MQTT Server is publicly accessible. Then the attacker can control your home from any place.

(3)
It might happen, that e.g. your Samsung SmartTV is not as secure as it should be and an attacker gets access to your network. Now he can listen to any traffic and maybe can make changes on all of your IoT devices.

(4)
If someone uses your device to spam mail or do a DOS attack the impact at your home is minimal. You might have more outbound traffic, but maybe you don't recognize this either. But thousands of hacked IoT devices can generate tremendous trouble even at the largest internet providers.

I hope these four typical scenarios ( the list is not complete) give you some idea, why you should take care, even if you're not a terrorist and normally nobody is interested into hacking you personally.

## Securing your WLAN for IoT
TBD


## Securing your communication for IoT
TBD

## Prevent to become part of a botnet
TBD

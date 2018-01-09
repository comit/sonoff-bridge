For those knowing more about HomeSeer please update this page.

**About HomeSeer**
``
HS3 is the industry standard for flexible, powerful, home automation software. A wide selection of software drivers (plug-ins) is available for use with scores of home automation technologies and products.
``

The following [forum link](https://forums.homeseer.com/showpost.php?p=1335412&postcount=60) provides a guide to upload Tasmota to an S20 using SonOTA and integrate it with HomeSeer HS3.

Requirements for HomeSeer HS3 and Tasmota devices:
* HomeSeer HS3 
* MQTT server
* A MQTT plugin for HS3

Currently there are two plugins, both free: "MQTT" and "mcsMQTT". 
The former is more intuitive but hasn't been updated for a while, the latter is newer and constantly updated.

If you use "MQTT" plugin you need to synch the virtual device to reflect the status of the physical button, this can be done  with a plugin:
* EasyTrigger plugin - costs 25$ (used to synchronise the status of the virtual device in HomeSeer when the Sonoff Tasmota module is operated from the physical button)

If you use "mcsMQTT", starting from ver 3.0.3+ it allows to create a device that both report and control the status of the Sonoff. More info here: https://forums.homeseer.com/showthread.php?t=192675


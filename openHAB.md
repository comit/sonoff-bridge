The "open Home Automation Bus" (openHAB) is an open source, technology agnostic home automation platform which runs as the center of your smart home. Besides 150 other add-ons for all kinds of technologies, openHAB provides an MQTT add-on ("binding") to interface with systems like Sonoff-Tasmota.

## openHAB Integration

By following the guide below you'll be able to observe, control and manage your Sonoff modules from your openHAB system. If you are new to openHAB, please learn about the basic concepts and the initial setup. The below article will not cover any basics which are out of scope to the Sonoff-Tasmota integration.

![example openHAB sitemap](https://community-openhab-org.s3-eu-central-1.amazonaws.com/original/2X/5/57750c6c7b6d9f18e75424fcb87ec093f70c6211.png "openHAB example of the end result shown in BasicUI")

### Requirements

* Working openHAB installation (http://docs.openhab.org)

* Configured Sonoff-Tasmota module (i.e. module accessible from your local network)

* MQTT broker available (e.g. Eclipse Mosquitto via [openHABian](http://docs.openhab.org/installation/openhabian.html))

* A [basic understanding of MQTT](http://www.hivemq.com/blog/mqtt-essentials) 

* Working and tested connection between openHAB and the MQTT broker

* (optional) Standalone [MQTT client](http://www.hivemq.com/blog/seven-best-mqtt-client-tools) (e.g. [mqtt-spy](https://kamilfb.github.io/mqtt-spy)) to observe and identify messages on the MQTT broker

**Highly recommeded:** If you are new to openHAB+MQTT, go through the [MQTT Binding Getting Started 101 tutorial](https://community.openhab.org/t/mqtt-binding-v1-11-getting-started-101/33958)!


If not done yet, you first need to **install and activate** the [MQTT binding](http://docs.openhab.org/addons/bindings/mqtt1/readme.html), the [MQTT action](http://docs.openhab.org/addons/actions/mqtt/readme.html) and the [JsonPath transformation](http://docs.openhab.org/addons/transformations/jsonpath/readme.html), e.g. via the openHAB Paper UI Add-ons section.

Before continuing, please make sure you assigned **unique MQTT "Topics"** in the Sonoff-Tasmota configuration interface of each Sonoff module. The default MQTT topic is "sonoff", in the examples below we will use names like "sonoff-A00EEA".

![Example Sonoff-Tasmota MQTT settings](https://community-openhab-org.s3-eu-central-1.amazonaws.com/original/2X/8/8fe9008fb24b0b70e6eddf7cf0f0c70c8ac21b92.png "Example Sonoff-Tasmota MQTT settings")

In the example configuration you can see a non-default **Full Topic** definition, which is **not** used in the following examples (but which can be recommended).

### Integration

Simply **set up items** for all Sonoff-Tasmota [MQTT topics](https://github.com/arendst/Sonoff-Tasmota/wiki/MQTT-Features) you are interested in. Examples for most needed topics are given below. Some Sonoff-Tasmota topics are JSON encoded, the `JSONPATH` transformation can be used to extract this data.
 
Additional or further interesting topics are easily identified by reading up on the Sonoff-Tasmota wiki and by subscribing to the modules topics. Subscribe to all topics of one module with the [MQTT wildcard](http://www.hivemq.com/blog/mqtt-essentials-part-5-mqtt-topics-best-practices) topic string `+/sonoff-XYZ/#` (String depends on your user-configured Topic/FullTopic). Configure items for the identified topics similar to the ones below.

**Example:**
MQTT messages published by a Sonoff Pow module are shown below (using [mosquitto_sub](https://mosquitto.org/man/mosquitto_sub-1.html)).
The module reports its device state and energy readings periodically.
In the second half of the example the module relay was switched into the OFF position.

```java
$ mosquitto_sub -h localhost -t "+/sonoff-E8A6E4/#" -v

tele/sonoff-E8A6E4/LWT Online
tele/sonoff-E8A6E4/UPTIME {"Time":"2017-07-25T12:02:00", "Uptime":68}
tele/sonoff-E8A6E4/STATE {"Time":"2017-07-25T12:06:28", "Uptime":68, "Vcc":3.122, "POWER":"POWER", "Wifi":{"AP":1, "SSID":"HotelZurBirke", "RSSI":100, "APMac":"24:65:11:BF:12:D8"}}
tele/sonoff-E8A6E4/ENERGY {"Time":"2017-07-25T12:06:28", "Total":0.640, "Yesterday":0.007, "Today":0.003, "Period":0, "Power":0, "Factor":0.00, "Voltage":0, "Current":0.000}
tele/sonoff-E8A6E4/STATE {"Time":"2017-07-25T12:11:28", "Uptime":68, "Vcc":3.122, "POWER":"POWER", "Wifi":{"AP":1, "SSID":"HotelZurBirke", "RSSI":100, "APMac":"24:65:11:BF:12:D8"}}
tele/sonoff-E8A6E4/ENERGY {"Time":"2017-07-25T12:11:28", "Total":0.640, "Yesterday":0.007, "Today":0.003, "Period":0, "Power":0, "Factor":0.00, "Voltage":0, "Current":0.000}
cmnd/sonoff-E8A6E4/POWER OFF
stat/sonoff-E8A6E4/RESULT {"POWER":"OFF"}
stat/sonoff-E8A6E4/POWER OFF
```

Following this method, the behavior-linked messages can be identified and bound to openHAB items.

### Mandatory Topics / Items

This it the minimal set of items for the basic functionality of different Sonoff modules. You'll need to replace the given example dive name (e.g. "sonoff-A00EEA") by the one chosen for your module. 
<br /> (*Note: Lines have been wrapped for better presentation*)

**sonoff.items:**

* Sonoff Basic / Sonoff S20 Smart Socket (Read and switch on-state)
  ```java
  Switch LivingRoom_Light "Living Room Light" <light> (LR,gLight)
      { mqtt=">[broker:cmnd/sonoff-A00EEA/POWER:command:*:default],
              <[broker:stat/sonoff-A00EEA/POWER:state:default]" }
  ```
* Sonoff Pow (Read and switch on-state, read current wattage)
  ```java
  // compare with example message stream above!
  Switch BA_Washingmachine "Washingmachine" <washer> (BA)
      { mqtt=">[broker:cmnd/sonoff-E8A6E4/POWER:command:*:default],
              <[broker:stat/sonoff-E8A6E4/POWER:state:default]" }
  
  Number BA_Washingmachine_Power "Washingmachine Power [%.1f W]" (BA,gPower)
      { mqtt="<[broker:tele/sonoff-E8A6E4/ENERGY:state:JSONPATH($.Power)]" }
  ```

### Status Topics / Items

It is furthermore recommended, to add the following status items for every Sonoff-Tasmota device.

**sonoff.items:** 

* A switch being 'ON' as long as the device is reachable 💬
  ```java
  Switch LivingRoom_Light_Reachable "Living Room Light: reachable" (gReachable)
      { mqtt="<[broker:tele/sonoff-A00EEA/LWT:state:MAP(reachable.map)]" }
  ```

* Wifi Signal Strength in Percent
  ```java
  Number LivingRoom_Light_RSSI "Living Room Light: RSSI [%d %%]" (gRSSI)
      { mqtt="<[broker:tele/sonoff-A00EEA/STATE:state:JSONPATH($.Wifi.RSSI)]" }
  ```

* Optional! A collection of return messages by the Sonoff module
<br>Recommendation: Define specific items for what you really need on a regular basis, use standalone MQTT client for troubleshooting
  ```java
  String LivingRoom_Light_Verbose "Living Room Light: MQTT return message [%s]"
      { mqtt="<[broker:tele/sonoff-A00EEA/INFO1:state:default],
              <[broker:stat/sonoff-A00EEA/STATUS2:state:default],
              <[broker:stat/sonoff-A00EEA/RESULT:state:default]" }
  ```

💬 The "LWT" topic (["Last Will and Testament"](http://www.hivemq.com/blog/mqtt-essentials-part-9-last-will-and-testament)) will receive regular "Online" messages by the module and an "Offline" message a short time after the module is disconnected, generated by the MQTT broker. These messages are transformed to a valid `ON`/`OFF` state by the [MAP](http://docs.openhab.org/addons/transformations/map/readme.html) transformation. Of course you can implement `Unreachable` instead of `Reachable` if you prefer. The following transformation file is needed:

**reachable.map:**
```java
Online=ON
Offline=OFF
```

## Maintenance Actions

A home automation system setup would not be complete without a certain maintenance automation!

Add the following elements to your openHAB setup to be able to perform actions on your Sonoff modules by the press of a simple sitemap button.

The example below includes upgrading the firmware of all modules. A shoutout to @evilgreen for the idea and a big thanks to @smadds for [providing](https://github.com/arendst/Sonoff-Tasmota/issues/19) the public firmware server used in the example.

![Sonoff Maintenance Actions](https://community-openhab-org.s3-eu-central-1.amazonaws.com/original/2X/9/97f0bdf6a81ffe94068e596804adf94839a5580b.png)

**sonoff.items:**
```java
//... all the above

//Maintenance
String	Sonoff_Action "Sonoff Action" <sonoff_basic>
```

**yourhome.sitemap:**
```java
//...
Switch item=Sonoff_Action mappings=[restart="Restart", queryFW="Query FW", upgrade="Upgrade FW"]
//...
```

**sonoff.rules:**
```java
// Work with a list of selected Sonoff modules
val sonoff_device_ids = newArrayList(
    "sonoff-A00EEA",
    //... add all your modules here!
    "sonoff-E8A6E4"
)
// OR
// Work with the grouptopic, addressing ALL modules at once
//val sonoff_device_ids = newArrayList("sonoffs")

rule "Sonoff Maintenance"
when
    Item Sonoff_Action received command
then 
    logInfo("sonoff.rules", "Sonoff Maintenance on all devices: " + receivedCommand)
    for (String device_id : sonoff_device_ids) {
        switch (receivedCommand) {
            case "restart" :
                publish("broker", "cmnd/" + device_id + "/restart", "1") 
            case "queryFW" :
                publish("broker", "cmnd/" + device_id + "/status", "2")
            case "upgrade" : {
                publish("broker", "cmnd/" + device_id + "/otaurl", "http://sonoff.maddox.co.uk/tasmota/sonoff.ino.bin")
                publish("broker", "cmnd/" + device_id + "/upgrade", "1")
            }
        }
    }
    Sonoff_Action.postUpdate(NULL)
end
```


## Community Forum

For more openHAB related details and questions, please visit the [openHAB community forum thread on Sonoff and Sonoff-Tasmota](https://community.openhab.org/t/itead-sonoff-switches-and-sockets-cheap-esp8266-wifi-mqtt-hardware/15024/1).
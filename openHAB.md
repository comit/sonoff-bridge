The open Home Automation Bus (openHAB) is an open source, technology agnostic home automation platform which runs as the center of your smart home. Besides 150 other add-ons for all kinds of technologies, openHAB provides an [MQTT add-on](http://docs.openhab.org/addons/bindings.html) ("binding") to interface with systems like Sonoff-Tasmota.

## openHAB Integration

After activating the MQTT binding, simply set up items for all Sonoff-Tasmota MQTT topics you are interested in. Examples are given below. Some Sonoff-Tasmota topics are JSON encoded. The `JSONPATH` transformation can be used to extract data.

### Mandatory Topics / Items

These are the minimal set of items for the basic functionality of different Sonoff modules. <br /> (*Note: Lines have been wrapped for better presentation*)

**sonoff.items:**
```java
//Sonoff Basic / Sonoff S20 Smart Socket
Switch LivingRoom_Corner_Light "Indirect Corner Light" <light> (LR,gLight)
    { mqtt=">[broker:cmnd/sonoff_A00F9D/power:command:*:default],
            <[broker:stat/sonoff_A00F9D/POWER:state:default]" }

// Sonoff Pow
Switch BA_Washingmachine "Washingmachine" <washer> (BA)
    { mqtt=">[broker:cmnd/sonoff_E8A6E4/power:command:*:default],
            <[broker:stat/sonoff_E8A6E4/POWER:state:default]" }
Number BA_Washingmachine_Power "Washingmachine Power [%.1f W]" (BA,gPower)
    { mqtt="<[broker:tele/sonoff_E8A6E4/TELEMETRY:state:JSONPATH($.Energy.Power)]" }
```

### Maintenance Topics / Items

It is furthermore recommended, to add the following maintenance items for every Sonoff-Tasmota device.

**sonoff.items:** 
```java
// Wifi Signal Strength in Percent
Number LivingRoom_Corner_Light_RSSI "Indirect Corner Light RSSI [%d %%]" (gRSSI)
    { mqtt="<[broker:tele/sonoff_A00F9D/TELEMETRY:state:JSONPATH($.Wifi.RSSI)]" }

// A switch turning ON if the device is unreachable
Switch LivingRoom_Corner_Light_Unreach "Indirect Corner Light unreachable" (gUnreach)
    { mqtt="<[broker:tele/sonoff_A00F9D/LWT:state:MAP(unreach.map)]" }
```

The "LWT" topic ("Last Will and Testament") will receive regular "Online" messages by the module and an "Offline" message a short time after the module is disconnected. These messages are transformed to a valid `ON`/`OFF` state by the MAP transformation. The following transformation file is needed.

**unreach.map:**
```java
Online=OFF
Offline=ON
```

Of course you can define `Reachable` instead of `Unreachable` if you prefer. 

## Community Forum

For more openHAB related details and questions, please visit the [openHAB community forum thread on Sonoff and Sonoff-Tasmota](https://community.openhab.org/t/itead-sonoff-switches-and-sockets-cheap-esp8266-wifi-mqtt-hardware/15024/1).
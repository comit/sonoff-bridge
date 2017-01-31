The open Home Automation Bus (openHAB) is an open source, technology agnostic home automation platform which runs as the center of your smart home. Besides 150 other add-ons for all kinds of technologies, openHAB provides an [MQTT add-on](http://docs.openhab.org/addons/bindings.html) ("binding") to interface with systems like Sonoff-Tasmota.

## openHAB Integration

After activating the MQTT binding, simply set up items for all Sonoff-Tasmota MQTT topics you are interested in. Examples are given below. Most Sonoff-Tasmota topics are JSON encoded. The `JSONPATH` transformation can be used to extract data. See the RSSI item.

**sonoff.items:**
```java
//Sonoff Basic / Sonoff S20 Smart Socket
Switch LivingRoom_Corner_Light "Indirect Corner Light" <light> (LR,gLight)
    { mqtt=">[broker:cmnd/sonoff_A00F9D/power:command:*:default],
            <[broker:stat/sonoff_A00F9D/POWER:state:default]" }
Number LivingRoom_Corner_Light_RSSI "Indirect Corner Light RSSI [%d %%]" (gRSSI)
    { mqtt="<[broker:tele/sonoff_A00F9D/TELEMETRY:state:JSONPATH($.Wifi.RSSI)]" }

// Sonoff Pow
Switch BA_Washingmachine "Washingmachine" <washer> (BA)
    { mqtt=">[broker:cmnd/sonoff_E8A6E4/power:command:*:default],
            <[broker:stat/sonoff_E8A6E4/POWER:state:default]" }
Number BA_Washingmachine_Power "Washingmachine Power [%d W]" (gRSSI)
    { mqtt="<[broker:tele/sonoff_E8A6E4/TELEMETRY:state:JSONPATH($.Energy.Power)]" }
Number BA_Washingmachine_RSSI "Washingmachine RSSI [%d %%]" (gRSSI)
    { mqtt="<[broker:tele/sonoff_E8A6E4/TELEMETRY:state:JSONPATH($.Wifi.RSSI)]" }
```

More examples to follow.

For more details and questions, please visit the [openHAB community forum thread on Sonoff and Sonoff-Tasmota](https://community.openhab.org/t/itead-sonoff-switches-and-sockets-cheap-esp8266-wifi-mqtt-hardware/15024/1)
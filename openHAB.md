The open Home Automation Bus (openHAB) is an open source, technology agnostic home automation platform which runs as the center of your smart home. Besides 150 other add-ons for all kinds of technologies, openHAB provides an [MQTT add-on](http://docs.openhab.org/addons/bindings.html) ("binding") to interface with systems like Sonoff-Tasmota.

## openHAB Integration

After activating the MQTT binding, simply set up items for all Sonoff-Tasmota MQTT topics you are interested in. Examples are given below

**sonoff.items:**
```java
//Sonoff Basic
Switch LivingRoom_Corner_Light "Indirect Corner Light" <light> (gLight)
    { mqtt=">[broker:cmnd/sonoff/power:command:*],
            <[broker:stat/sonoff/POWER:state:default" }
```

More examples to follow.

For more details and questions, please visit the [openHAB community forum thread on Sonoff and Sonoff-Tasmota](https://community.openhab.org/t/itead-sonoff-switches-and-sockets-cheap-esp8266-wifi-mqtt-hardware/15024/1)
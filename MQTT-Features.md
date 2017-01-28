Sonoff responds to the following MQTT commands using MQTT Topic for ```cmnd/sonoff/<command>``` and MQTT Message/Payload for ```<parameter>```:

- the relay can be controlled with ```cmnd/sonoff/power on```, ```cmnd/sonoff/power off``` or ```cmnd/sonoff/power toggle```. The LED will blink twice and sends a MQTT status message like ```stat/sonoff/POWER ON```. The same function can be initiated with ```cmnd/sonoff/light on```.

- The power state message can be sent with the retain flag set. Enable this with ```cmnd/sonoff/PowerRetain on```.

- For sonoff dual the relays need to be addressed with ```cmnd/sonoff/power1 toggle``` and ```cmnd/sonoff/power2 toggle```.

- the MQTT topic can be changed with ```cmnd/sonoff/topic sonoff1``` which reboots sonoff and makes it available for MQTT commands like ```cmnd/sonoff1/power on```.

- the OTA firmware location can be made known to sonoff with ```cmnd/sonoff/otaurl http://domus1:80/api/sonoff/user1.bin``` where domus1 is your webserver hosting the firmware. Reset to default with ```cmnd/sonoff/otaurl 1```.

- upgrade OTA firmware with ```cmnd/sonoff/upgrade 1```.

- show all status information with ```cmnd/sonoff/status 0```.

- The button can send a MQTT message to the broker that in turn will switch the relay. To configure this you need to perform ```cmnd/sonoff/ButtonTopic sonoff``` where sonoff equals to Topic. The message can also be provided with the retain flag by ```cmnd/sonoff/ButtonRetain on```.

- Sonoff Pow status can be retreived with ```cmnd/sonoff/status 8``` or periodically every 5 minutes using ```cmnd/sonoff/TelePeriod 300```. If units appended to the message are needed execute command ```cmnd/sonoff/Units on```.

- When a Sonoff Pow threshold like PowerLow has been met a message ```tele/sonoff/POWER_LOW ON``` will be sent. When the error is corrected a message ```tele/sonoff/POWER_LOW OFF``` will be sent.

While most MQTT commands will result in a message in JSON format the power status feedback will always be returned like ```stat/sonoff/POWER ON``` too.

Telemetry data will be sent by prefix ```tele``` like ```tele/sonoff/TELEMETRY {"Temperature":"24.7"}```

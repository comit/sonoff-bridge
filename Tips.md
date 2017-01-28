Find below some background and other information.

### Topic, GroupTopic and FallBack Topic
Initially I had one MQTT configurable topic planned called TOPIC but soon found out that when two sonoffs come online with the same topic this would be a challenge at least...

I then introduced a unique, non-configurable topic which I call fallback topic that allows me to always change the MQTT configurable topic to a new unique topic. This fallback topic is just what it is meant to be: A FALLBACK TOPIC in case of emergency.

All MQTT status messages will be sent using the configurable TOPIC which should be made unique by the user. It might be called  bedroom  but it could also have been called  titanic10  as long as the user knows what it is and where to find it.

Having two sonoffs with the same topic allowed for MQTT commands to be sent once to make them act in sono. That inspired me to add a third topic to subscribe to which I call GROUPTOPIC. Sonoffs with the same GROUPTOPIC will react to the same MQTT command. I use it to update firmware to all my sonoffs using separate groups for plain sonoff, pow sonoffs and th sonoffs.

BTW changing TOPIC can be done online using the fallback topic and is only needed once. There is no need to change user_config.h all the time, as many users seem to think they have to do with any new release. All changes are stored in flash and I make a lot of effort to keep these changes available between versions.

### For Flash afficionados
- To stop saving parameter changes to Flash or Spiffs use command ```SaveData off```.

- To stop saving power changes only to Flash or Spiffs use command ```SaveState off```. This will disable the relay from returning to the same state after power on UNLESS you use the MQTT retain flag in which case the MQTT broker will send the last known MQTT state on restart or power on. The command ```ButtonRetain on``` will configure the button to send a MQTT command with Topic and the MQTT retain flag set.

### Debugging
- For debugging purposes you can use the serial interface with command ```SerialLog 4``` and the Arduino IDE set to 115200 baud (19200 for Sonoff Dual) and both NL & CR or the web console with command ```WebLog 4```.

- To aid in finding the IP address of sonoff the network name will be ```<MQTT_TOPIC>-<last 4 decimal chars of MAC address>```. So the default name is ```sonoff-1234```. Another option is MQTT command ```Status 5```.

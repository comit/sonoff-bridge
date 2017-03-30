The standard Sonoff device has a relay to turn on/off an external device and a button to toggle the state of the relay.

By default the configuration for the button looks like this:

```
SwitchMode=0 (Set switch mode to TOGGLE)
SwitchTopic=0 (Disable use of MQTT switch topic) 
```

`SwitchMode=0` means every time the button gets pressed the relay changes its state (on/off).

`SwitchTopic=0` means the button press will not send a topic via MQTT.
Instead the relay will be controlled directly. Once the relay changes its state the firmware will send an MQTT message with the new state.  
e.g.: `stat/sonoff01/POWER ON`

**Important**:  
The Sonoff-Tasmota firmware does not publish the state of the switch in any way!
It only send commands (directly to the corresponding relay or via MQTT to a topic).


# SwitchMode

You can change the mode for a switch by executing a [command](Commands) in the web console, via HTTP, via MQTT or via serial.

## SwitchMode 1 and 2

**SwitchMode 1**

> Set switch mode to FOLLOW (0 = Off, 1 = On)

While pressing the button/switch (closing the circuit) the button will send `ON` (directly or via MQTT depending on SwitchTopic) and `OFF` when released (open circuit).


**SwitchMode 2**

> Set switch mode to inverted FOLLOW (0 = On, 1 = Off)

The opposite of Mode 1.

### Conclusion

You properly want to use `SwitchMode=1` when connecting a [toggle switch](https://en.wikipedia.org/wiki/Switch#Toggle_switch) to your Sonoff device. This way the 'software switch' will have the same state as the 'hardware switch'.
If the real switch is in the "on" position, the state of the software switch is `ON` as well.

## SwitchMode 3 and 4

**SwitchMode 3**

> Set switch mode to PUSHBUTTON (Normally 1, 0 = toggle)

The switch will send a command (directly or via MQTT) when the switch opens the circuit (`OFF`). When closing the circuit (`ON`), nothing will happen.

**SwitchMode 4**

> Set switch mode to inverted PUSHBUTTON (Normally 0, 1 = toggle)

The switch will send a command when changing to `ON`. Nothing happens when changing to `OFF`.


### Conclusion

When connecting a [momentary switch](https://en.wikipedia.org/wiki/Switch#Biased_switches) (a push button) you want to use `SwitchMode=3` or `4`.  
`SwitchMode=4` is the same as the default behavior (`SwitchMode=0`) for Sonoff devices.

# SwitchTopic

## SwitchTopic 0

> Disable use of MQTT switch topic

No MQTT message will be published on account of the new switch state. The message you see in MQTT is the new state of the relay that gets changed, like explained above.
e.g.: `stat/sonoff01/POWER ON`

## SwitchTopic 1

> Set MQTT switch topic to Topic

When changing the state of the switch (depending on the `SwitchMode`) an MQTT command gets send via MQTT.
This command will have the topic defined as the default topic of the Sonoff device.  
e.g.: `cmnd/sonoff01/POWER ON` (Notice the `cmnd` instead of the `stat` at the beginning.)  
This command will be received by the device itself and the relay will be set to the defined state.
Just like receiving the same command from any other MQTT source.

## SwitchTopic 2

> Set MQTT switch topic (32 chars max)

This will send a MQTT message with a user-defined topic.
Weather this message will trigger anything depends on the setup of the user.
For example, when setting the topic to `sonoff2` this could trigger the relay on the Sonoff device that listens to messages with this specific topic.

## Use case
To trigger something within your smart home setup (e.g. [openHAB]) you can define the SwitchTopic to `sonoff01-switch1` (for example).

This will send a message like this: `cmnd/sonoff01-switch1/POWER ON`

In openhab you define an item that subscribes to this topic and sets its state according to the message content.

```java
Switch Sonoff01_Switch1 "Sonoff01 Switch1 [%s]" <switch> { mqtt="<[mosquitto:cmnd/sonoff01-switch1/POWER1:state:default]" }
```

## Conclusion

`SwitchMode=0` controls the relay directly.  
`SwitchMode=1` sends a message with the default topic to MQTT. This message will get picked up by the device itself and sets the state of the relay accordingly.  
While `SwitchMode=2` sends a command with a custom topic via MQTT. This will not get picked up unless you configure any device to subscribe to this topic.


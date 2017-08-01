## Predefined PowerOnState functionality

`PowerOnState     |              | Show current relay power on state`  
`PowerOnState     | 0 | off      | Keep relay(s) off after power on`  
`PowerOnState     | 1 | on       | Turn relay(s) on after power on`  
`PowerOnState     | 2 | toggle   | Toggle relay(s) on from last saved`  
`PowerOnState     | 3            | (default) Turn relay(s) on as last saved`  
`PowerOnState     | 4            | Turn relay(s) on and disable further relay control`

by DEFAULT the relay(s) will power on with the last saved state on a reboot/restart.

## Side effects with using MQTT messages

If MQTT is defined and PowerRetain is used the last state will be stored permanently in MQTT database.

`PowerRetain  |              | Show current MQTT power retain state`  
`PowerRetain  | 0 | off      | (default) Disable MQTT power retain on status update`  
`PowerRetain  | 1 | on       | Enable MQTT power retain on status update`

The stored MQTT message will always **override the PowerOnState!!!**

To check, if there is a value set for the power switch you can use mosquitto_sub

`mosquitto_sub -p 1883 -u <username> -P <password> -t cmnd/+/power`

If there are retained messages there should be an output similar to: (the "r1" shows that this message was retained)

`Client mosqsub/3795-raspberryp received PUBLISH (d0, q0, r1, m0, 'cmnd/setting/ESP_004554/power', ... (1 bytes))`  
`on`  
`Client mosqsub/3795-raspberryp received PUBLISH (d0, q0, r1, m0, 'cmnd/setting/ESP_267688/power', ... (1 bytes))`  
`off`  
`Client mosqsub/3795-raspberryp received PUBLISH (d0, q0, r1, m0, 'cmnd/setting/ESP_675667/power', ... (1 bytes))`  
`on`


cmnd/+/power could also something different depending what you have defined as IN folder for commands. "power" needs to be replaced by power1,power2 and so on, if you have more relays or use "cmnd/#". Be aware that MQTT does **NOT SUPPORT **wildcards "cmnd/+/Power?"

`#define SUB_PREFIX             "cmnd" `

Now you can see, if there are messages stored that Power ON/OFF and need to be deleted. To delete use:

`mosquitto_pub -p 1883 -u <username> -P <password> -d -n -r -t cmnd/ESP_0C220C/power`

The -n sends an empty key and the -r store this change permanently

## Enable relay always ON after power on

To make sure the relay stays on after power on you will have to do a small workaround. After setting up your sonoff do the falling:

1. Set PowerOnState to ON `http://<IP_Address_of_you_Sonoff>/cm?cmnd=PowerOnState%201`
2. Set PowerRetain to ON `http://<IP_Address_of_you_Sonoff>/cm?cmnd=PowerRetain%201`
3. Navigate to the web interface `http://<IP_Address_of_you_Sonoff>` and click **toggle** to turn the relay to ON. **If the relay is on toggle it OFF and back ON.**
4. Set PowerRetain to OFF `http://<IP_Address_of_you_Sonoff>/cm?cmnd=PowerRetain%200`

## Enable relay always OFF after power on

To make sure the relay stays on after power on you will have to do a small workaround. After setting up your sonoff do the falling:

1. Set PowerOnState to OFF `http://<IP_Address_of_you_Sonoff>/cm?cmnd=PowerOnState%200`
2. Set PowerRetain to On `http://<IP_Address_of_you_Sonoff>/cm?cmnd=PowerRetain%201`
3. Navigate to the web interface `http://<IP_Address_of_you_Sonoff>` and click **toggle** to turn the relay to OFF. **If the relay is on toggle it ON and back OFF.**
4. Set PowerRetain to Off `http://<IP_Address_of_you_Sonoff>/cm?cmnd=PowerRetain%200`


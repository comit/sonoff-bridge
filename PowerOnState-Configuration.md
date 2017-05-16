## Predefined PowerOnState functionality

PowerOnState     |              | Show current relay power on state
PowerOnState     | 0 | off      | Keep relay(s) off after power on
PowerOnState     | 1 | on       | Turn relay(s) on after power on
PowerOnState     | 2 | toggle   | Toggle relay(s) on from last saved
PowerOnState     | 3            | (default) Turn relay(s) on as last saved
PowerOnState     | 4            | Turn relay(s) on and disable further relay control

by DEFAULT the relay(s) will power on with the last saved state on a reboot/restart.

## Side effects with using MQTT messages

If MQTT is defined and PowerRetain is used the last state will be stored permanently in MQTT database.

PowerRetain  |              | Show current MQTT power retain state
PowerRetain  | 0 | off      | (default) Disable MQTT power retain on status update
PowerRetain  | 1 | on       | Enable MQTT power retain on status update

The stored MQTT message will always **override the PowerOnState!!!**

To check, if there is a value set for the power switch you can use mosquitto_sub

mosquitto_sub -p 1883 -u <username> -P <password> -t hm/setting/#

hm/setting/# could also something different depending what you have defined as IN folder for commands

#define SUB_PREFIX             "cmnd" 

Now you can see, if there are messages stored that Power ON/OFF and need to be deleted. To delete use:

mosquitto_pub -p 1883 -u <username> -P <password> -d -n -r -t hm/setting/ESP_0C220A/power

The -n sends an empty key and the -r store this change permanently



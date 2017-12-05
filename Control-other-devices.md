## Douple press & hold

It is possible, to control another device from remote by using following options:

From version 5.1.6 on, hold button functionality for both the push button AND the external pushbutton was implemented. If a ButtonTopic (and if SetOption1 (=ButtonRestrict) is On) or SwitchTopic (and SwitchMode 5|6) has been defined and a button is pressed longer than define KEY_HOLD_TIME (in 0.1 seconds. default 4 seconds) a MQTT message like cmnd/sonoff/POWER HOLD will be send. The HOLD text can be changed with command StateText4.

Another command (SetOption11 On|Off) allows for swapping the functionality of the push button.

These changes result in below table

### Action Matrix

| Set<br>Option1 | Set<br>Option11 | Button<br>Topic    | Single Press | Double Press | Press Hold | Description   |
|---------|----------|----------|--------------|-------------|------------|-----------------------------|
| 0       | 0        | 0        | toggle relay | ditto       | Reset      | ditto used for Dual relay 2 |
| 0       | 0        | = topic  | send MQTT msg:<br>cmnd/\<topic\>/POWER ON\|OFF| toggle relay | Reset 1  | double press backup for MQTT failure |      
| 0       | 0        | <> topic | send MQTT msg:<br>cmnd/\<non-topic\>/POWER TOGGLE | toggle relay                   | Reset 1                     | double press backup for MQTT failure |
|---------|----------|----------|--------------|-------------|------------|-----------------------------|
| 1       | 0        | 0        | toggle relay                  | ditto                          | discarded                   | ditto used for Dual relay 2          |
| 1       | 0        | = topic  | send MQTT msg:<br>cmnd/\<topic\>/POWER ON\|OFF| toggle relay                   | send MQTT msg: cmnd/\<topic\>/POWER HOLD | double press backup for MQTT failure |
| 1       | 0        | \<\> topic | Send MQTT msg:<br>cmnd/\<non-topic\>/POWER TOGGLE| toggle relay                   | send MQTT msg: cmnd/\<non-topic\>/POWER HOLD | double press backup for MQTT failure |
|---------|----------|----------|--------------|-------------|------------|-----------------------------|
| 0       | 1        | 0        | toggle relay                  | ditto                          | Reset 1                     | ditto used for Dual relay 2          |
| 0       | 1        | = topic  | toggle relay                  | send MQTT msg:<br>cmnd/\<topic\>/POWER ON\|OFF | Reset 1                     | single press for MQTT failure        |
| 0       | 1        | \<\> topic | toggle relay                  | send MQTT msg:<br>cmnd/\<non-topic\>/POWER TOGGLE | Reset 1                     | single press for MQTT failure        |
|---------|----------|----------|--------------|-------------|------------|-----------------------------|
| 1       | 1        | 0        | toggle relay                  | ditto                          | discarded                   | ditto used for Dual relay 2          |
| 1       | 1        | = topic  | toggle relay                  | send MQTT msg:<br>cmnd/\<topic\>/POWER ON\|OFF | send MQTT msg: cmnd/\<topic\>/POWER HOLD | single press for MQTT failure        |
| 1       | 1        | <> topic | toggle relay                  | send MQTT msg:<br>cmnd/\<non-topic\>/POWER TOGGLE | send MQTT msg:<br>cmnd/\<non-topic\>/POWER HOLD | single press for MQTT failure        |

***

### Example

You can control for instance a ceiling fan from a sonoff touch:<br>
If your standard topic at the sonoff touch is "light" and the topic at the ceiling's fan is "ceilingfan", then you can set "buttontopic ceilingfan" and "setoption11 1" at the sonoff touch, to activate the double press feature.

Taken from the discussion:
[https://github.com/arendst/Sonoff-Tasmota/issues/200#issuecomment-343756826](https://github.com/arendst/Sonoff-Tasmota/issues/200#issuecomment-343756826)

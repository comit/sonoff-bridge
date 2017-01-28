The firmware supports a **serial**, **MQTT** and **Web** Man Machine interface. The serial interface is set to 115200 bps except for Sonoff Dual where it is set to 19200 bps. The MQTT commands are constructed from MQTT Topic for ```cmnd/sonoff/<command>``` and MQTT Message for ```<parameter>``` and are NOT case sensitive. 

Commands are allowed like ```cmnd/sonoff/<command><relay>``` to address a specific relay where applicable.

The following command tables are available:
- Main
- [Management](#management)
- [Wifi](#wifi)
- [MQTT](#mqtt)
- [Logging](#logging)
- [Sonoff Pow specific](#sonoff-pow)
- [Domoticz](#domoticz)
- [WS2812 led string](#ws2812-led-string)
- [Next Generation specific](#next-generation)
- [Optional](#optional)

### Main
Command | Version | Description
------- | ------- | -----------
BlinkCount | | Show current BlinkCount
BlinkCount 0 | | Blink many times before restoring power state
BlinkCount 1 .. 32000 | | Set how many blinks before restoring power state
BlinkTime | | Show current BlinkTime in 0.1 seconds
BlinkTime 2 .. 3600 | | Set BlinkTime with 0.1 seconds increment
LedPower | | Show current led power state as On or Off
LedPower 0 \| off | | Turn led AND LedState Off
LedPower 1 \| on | | Turn led On AND LedState Off
LedState | | Show current led state as 0 to 7
LedState 0 \| off | | Disable use of LED as much as possible
LedState 1 \| on | | Show power state on led
LedState 2 | | Show MQTT subscriptions as a led blink
LedState 3 | | Show power state and MQTT subscriptions as a led blink
LedState 4 | | Show MQTT publications as a led blink
LedState 5 | | Show power state and MQTT publications as a led blink
LedState 6 | | Show all MQTT messages as a led blink
LedState 7 | | Show power state and MQTT messages as a led blink
Light | | Show current power state as On or Off
Light 0 \| off | | Turn power Off
Light 1 \| on | | Turn power On
Light 2 \| toggle | | Toggle power
Light 3 \| blink | | Blink power
Light 4 \| blinkoff | | Stop blinking power
Light\<x\> | | Show current power state of relay\<x\>
Light\<x\> 0 \| off | | Turn relay\<x\> power Off
Light\<x\> 1 \| on | | Turn relay\<x\> power On
Light\<x\> 2 \| toggle | | Toggle relay\<x\> power
Light\<x\> 3 \| blink | | Blink relay\<x\> power
Light\<x\> 4 \| blinkoff | | Stop blinking relay\<x\> power
Power | | Show current power state as On or Off
Power 0 \| off | | Turn power Off
Power 1 \| on | | Turn power On
Power 2 \| toggle | | Toggle power
Power 3 \| blink | | Blink power
Power 4 \| blinkoff | | Stop blinking power
Power\<x\> | | Show current power state of relay\<x\>
Power\<x\> 0 \| off | | Turn relay\<x\> power Off
Power\<x\> 1 \| on | | Turn relay\<x\> power On
Power\<x\> 2 \| toggle | | Toggle relay\<x\> power
Power\<x\> 3 \| blink | | Blink relay\<x\> power
Power\<x\> 4 \| blinkoff | | Stop blinking relay\<x\> power
PowerOnState | | Show current relay power on state
PowerOnState 0 | | Keep relay(s) off after power on
PowerOnState 1 | | Turn relay(s) on after power on
PowerOnState 2 | | Toggle relay(s) on from last saved
PowerOnState 3 | | (default) Turn relay(s) on as last saved
PowerRetain | | Show current MQTT power retain state
PowerRetain 0 \| off | | (default) Disable MQTT power retain on status update
PowerRetain 1 \| on | | Enable MQTT power retain on status update
PulseTime | | Show current PulseTime in 0.1 seconds
PulseTime 0 \| off | | (Default) Disable use of PulseTime
PulseTime 1 .. 111 | | Set PulseTime with 0.1 seconds increment
PulseTime 112 .. 64900 | | Set PulseTime with 1 seconds increment starting with 12 seconds (113 = 13 seconds etc.)

### Management
Command | Version | Description
------- | ------- | -----------
FriendlyName | | Show friendly name as used by emulation
FriendlyName\<x\> | | Show friendly name as used by emulation
FriendlyName\<x\> 1 | | Reset friendly name to ```user_config.h``` value (FRIENDLY_NAME)
FriendlyName\<x\> \<your-name\> | | Set friendly name
OtaUrl | | Show current otaurl
OtaUrl 1 | | Reset otaurl to ```user_config.h``` value
OtaUrl \<your-otaurl\> | | Set otaurl
Reset 1 | | Reset sonoff parameters to ```user_config.h``` values and restart
Reset 2 | | Erase flash, reset sonoff parameters to ```user_config.h``` values and restart
Restart 1 | | Restart sonoff
Restart 99 | | Force restart sonoff without config save
SaveData | | Save parameter changes and show current state as Manual, On or every x seconds
SaveData 0 \| off | | Save parameter changes only manually
SaveData 1 \| on | | (default) Save parameter changes every second
SaveData \<your-secs\> | 1.0.35 | Save parameter changes between every 2 and 3600 seconds
SaveState | | Show current SaveState state
SaveState 1 \| on | | (default) Save power changes and set relay after restart
SaveState 0 \| off | | Do not save power changes and do not set relay after restart
Sleep | | Show current sleep state as 0 (Off) or duration of up to 250 mSec
Sleep 0 \| off | | (default) Turn sleep off
Sleep 1 - 250 | | Set sleep duration from 1 to 250 mSec to enable energy saving
Status | | Show abbreviated status information
Status 0 | | Show all status information
Status 1 | | Show more status information
Status 2 | | Show firmware information
Status 3 | | Show syslog information
Status 4 | | Show memory information
Status 5 | | Show network information
Status 6 | | Show MQTT information
Status 7 | | Show Real Time Clock information
Status 8 | | (Sonoff Pow only) Show Power usage
Status 9 | | (Sonoff Pow only) Show Power thresholds
Timezone | | Show current timezone
Timezone -12 .. 12 | | Set timezone
Timezone 99 | | Use Daylight Saving parameters from ```user_config.h```
Upgrade 1 | | Download ota firmware from your web server and restart
Upload 1 | | Download ota firmware from your web server and restart

### Wifi
Command | Version | Description
------- | ------- | -----------
AP | 2.1.2 | Show current selected Wifi Access Point (AP)
AP 0 | 2.1.2 | Switch to other Wifi Access Point (AP)
AP 1 | 2.1.2 | Select Wifi Access Point 1 (AP)
AP 2 | 2.1.2 | Select Wifi Access Point 2 (AP)
Hostname | 1.0.26 | Show current hostname
Hostname 1 | 1.0.26 | Reset hostname to ```user_config.h``` value and restart
Hostname \<your-host\> | 1.0.26 | Set hostname and restart
Password | | Show AP1 current Wifi password
Password 1 | | Reset AP1 Wifi password to ```user_config.h``` value and restart
Password \<your-password\> | | Set AP1 Wifi password and restart
Password1 | 2.1.2 | Show AP1 current Wifi password
Password1 1 | 2.1.2 | Reset AP1 Wifi password to ```user_config.h``` value and restart
Password1 \<your-password\> | 2.1.2 | Set AP1 Wifi password and restart
Password2 | 2.1.2 | Show AP2 current Wifi password
Password2 1 | 2.1.2 | Reset AP2 Wifi password to ```user_config.h``` value and restart
Password2 \<your-password\> | 2.1.2 | Set AP2 Wifi password and restart
SmartConfig 1 | | (Deprecated - use wificonfig) Start smart config for 1 minute
SmartConfig 2 | 1.0.22 | (Deprecated - use wificonfig) Start wifi manager (web server at 192.168.4.1)
SSId | | Show AP1 current Wifi SSId
SSId 1 | | Reset AP1 Wifi SSId to ```user_config.h``` value and restart
SSId \<your-ssid\> | | Set AP1 Wifi SSId and restart
SSId1 | 2.1.2 | Show AP1 current Wifi SSId
SSId1 1 | 2.1.2 | Reset AP1 Wifi SSId to ```user_config.h``` value and restart
SSId1 \<your-ssid\> | 2.1.2 | Set AP1 Wifi SSId and restart
SSId2 | 2.1.2 | Show AP2 current Wifi SSId
SSId2 1 | 2.1.2 | Reset AP2 Wifi SSId to ```user_config.h``` value and restart
SSId2 \<your-ssid\> | 2.1.2 | Set AP2 Wifi SSId and restart
WebServer | 1.0.23 | Show current web server state
WebServer 0 | 1.0.23 | Stop web server
WebServer 1 | 1.0.23 | Start web server in user mode
WebServer 2 | 1.0.23 | Start web server in admin mode
WifiConfig | 1.0.32 | Show current config tool
WifiConfig 0 | 1.0.32 | (Deprecated) Start current config tool
WifiConfig 0 | 2.1.2 | Disable wifi config but restart (used with alternate AP)
WifiConfig 1 | 1.0.32 | Start smart config for 1 minute and set as current config tool
WifiConfig 2 | 1.0.32 | Start wifi manager (web server at 192.168.4.1) and set as current config tool
WifiConfig 3 | 1.0.32 | Start WPS config for 1 minute and set as current config tool
WifiConfig 4 | 3.0.5 | Disable wifi config but retry other AP without restart

### MQTT
Command | Version | Description
------- | ------- | -----------
ButtonRetain | 2.0.3 | Show current button MQTT retain flag state
ButtonRetain on | 2.0.3 | Set ButtonTopic to Topic and enable MQTT retain flag on button press
ButtonRetain off | 2.0.3 | (default) Disable use of MQTT retain flag
ButtonRetain 1 | 2.0.3 | Set ButtonTopic to Topic and enable MQTT retain flag on button press
ButtonRetain 0 | 2.0.3 | (default) Disable use of MQTT retain flag
ButtonTopic | 1.0.10 | Show current MQTT button topic
ButtonTopic 0 | 1.0.10 | Disable use of MQTT button topic
ButtonTopic 1 | 1.0.10 | Set MQTT button topic to Topic
ButtonTopic \<your-topic\> | 1.0.10 | Set MQTT button topic
GroupTopic | | Show current MQTT group topic
GroupTopic 1 | | Reset MQTT group topic to ```user_config.h``` value and restart
GroupTopic \<your-grouptopic\> | | Set MQTT group topic and restart
MessageFormat | 2.0.7 | (Until 4.0.0) Show current MQTT message format (0 = Legacy, 1 = JSON)
MessageFormat 0 | 2.0.7 | (Until 4.0.0) (default) Send legacy messages
MessageFormat 1 | 2.0.7 | (Until 4.0.0) Send JSON messages and Legacy power state message
MqttClient | 1.0.22 | Show current MQTT client
MqttClient 1 | 1.0.22 | Reset MQTT client to ```user_config.h``` value and restart
MqttClient \<your-client\> | 1.0.22 | Set MQTT client and restart. May use wildcard %06X to be replaced by last six characters of MAC address
MqttHost | | Show current MQTT host
MqttHost 1 | | Reset MQTT host to ```user_config.h``` value and restart
MqttHost \<your-host\> | | Set MQTT host and restart
MqttPassword | 1.0.22 | Show current MQTT password
MqttPassword 1 | 1.0.22 | Reset MQTT password to ```user_config.h``` value and restart
MqttPassword \<your-password\> | 1.0.22 | Set MQTT password and restart
MqttPort | 1.0.22 | Show current MQTT port
MqttPort 1 | 1.0.22 | Reset MQTT port to ```user_config.h``` value and restart
MqttPort \<your-port\> | 1.0.22 | Set MQTT port between 2 and 32766 and restart
MqttUser | 1.0.22 | (Until 4.0.0) Show current MQTT user name
MqttUser 1 | 1.0.22 | (Until 4.0.0) Reset MQTT user name to ```user_config.h``` value and restart
MqttUser \<your-user\> | 1.0.22 | (Until 4.0.0) Set MQTT user name and restart
MqttUnits | 2.0.5 | (Until 4.0.0) Show current MqttUnits state
MqttUnits on | 2.0.5 | (Until 4.0.0) Add units to MQTT messages
MqttUnits off | 2.0.5 | (Until 4.0.0) (default) Do not show units to MQTT messages
Units 1 | 4.0.0 | Add units to messages
Units 0 | 4.0.0 | (default) Do not show units to messages
Units | 4.0.0 | Show current Units state
Units on | 4.0.0 | Add units to messages
Units off | 4.0.0 | (default) Do not show units to messages
Units 1 | 4.0.0 | Add units to messages
Units 0 | 4.0.0 | (default) Do not show units to messages
PowerRetain | 3.0.0 | Show current MQTT power retain state
PowerRetain on | 3.0.0 | Enable MQTT power retain on status update
PowerRetain off | 3.0.0 | (default) Disable MQTT power retain on status update
PowerRetain 1 | 3.0.0 | Enable MQTT power retain on status update
PowerRetain 0 | 3.0.0 | (default) Disable MQTT power retain on status update
SwitchRetain | 3.2.9 | Show current button MQTT retain flag state
SwitchRetain on | 3.2.9 | Set ButtonTopic to Topic and enable MQTT retain flag on button press
SwitchRetain off | 3.2.9 | (default) Disable use of MQTT retain flag
SwitchRetain 1 | 3.2.9 | Set ButtonTopic to Topic and enable MQTT retain flag on button press
SwitchRetain 0 | 3.2.9 | (default) Disable use of MQTT retain flag
SwitchTopic | 3.2.9 | Show current MQTT button topic
SwitchTopic 0 | 3.2.9 | Disable use of MQTT button topic
SwitchTopic 1 | 3.2.9 | Set MQTT button topic to Topic
SwitchTopic \<your-topic\> | 3.2.9 | Set MQTT button topic
TelePeriod | 1.0.28 | Show current telemetry period in seconds
TelePeriod off | 1.0.28 | Disable telemetry messages
TelePeriod 0 | 1.0.28 | Disable telemetry messages
TelePeriod 1 | 1.0.28 | Reset telemetry period to ```user_config.h``` value
TelePeriod \<your-secs\> | 1.0.28 | Set telemetry period between 2 and 3600 seconds
Topic | | Show current MQTT topic
Topic 1 | | Reset MQTT topic to ```user_config.h``` value and restart
Topic \<your-topic\> | | Set MQTT topic  AND button topic and restart

### Logging
Command | Version | Description
------- | ------- | -----------
LogHost | 1.0.7 | Show current syslog host
LogHost 1 | 1.0.7 | Reset syslog host to ```user_config.h``` value
LogHost \<your-host\> | 1.0.7 | Set syslog host
LogPort | 1.0.26 | Show current syslog port
LogPort 1 | 1.0.26 | Reset syslog port to ```user_config.h``` value
LogPort \<your-port\> | 1.0.26 | Set syslog port between 2 and 32766
SerialLog 0 | 1.0.7 | Disable serial logging
SerialLog 1 | 1.0.7 | Show only error messages
SerialLog 2 | 1.0.7 | Show error and info messages
SerialLog 3 | 1.0.7 | Show error, info and debug messages
SerialLog 4 | 1.0.7 | Show all messages
SysLog 0 | 1.0.7 | Disable syslog logging
SysLog 1 | 1.0.7 | Show only error messages
SysLog 2 | 1.0.7 | Show error and info messages
SysLog 3 | 1.0.7 | Show error, info and debug messages
SysLog 4 | 1.0.7 | Show all messages
WebLog 0 | 1.0.27 | Disable web logging
WebLog 1 | 1.0.27 | Show only error messages
WebLog 2 | 1.0.27 | Show error and info messages
WebLog 3 | 1.0.27 | Show error, info and debug messages
WebLog 4 | 1.0.27 | Show all messages

### Sonoff Pow
Command | Version | Description
------- | ------- | -----------
CurrentHigh | 2.0.6 | Show current current high threshold value
CurrentHigh off | 2.0.6 | (default) Disable current high threshold
CurrentHigh 0 | 2.0.6 | (default) Disable current high threshold
CurrentHigh \<milliamps\> | 2.0.6 | Set current high threshold value
CurrentLow | 2.0.6 | Show current current low threshold value
CurrentLow off | 2.0.6 | (default) Disable current low threshold
CurrentLow 0 | 2.0.6 | (default) Disable current low threshold
CurrentLow \<milliamps\> | 2.0.6 | Set current low threshold value
PowerHigh | 2.0.6 | Show current power high threshold value
PowerHigh off | 2.0.6 | (default) Disable power high threshold
PowerHigh 0 | 2.0.6 | (default) Disable power high threshold
PowerHigh \<watt\> | 2.0.6 | Set power high threshold value
PowerLow | 2.0.6 | Show current power low threshold value
PowerLow off | 2.0.6 | (default) Disable power low threshold
PowerLow 0 | 2.0.6 | (default) Disable power low threshold
PowerLow \<watt\> | 2.0.6 | Set power low threshold value
Status 8 | 2.0.5 | Show Power usage
Status 9 | 2.0.6 | Show Power thresholds
VoltageHigh | 2.0.6 | Show current voltage high threshold value
VoltageHigh off | 2.0.6 | (default) Disable voltage high threshold
VoltageHigh 0 | 2.0.6 | (default) Disable voltage high threshold
VoltageHigh \<voltage\> | 2.0.6 | Set voltage high threshold value
VoltageLow | 2.0.6 | Show current voltage low threshold value
VoltageLow off | 2.0.6 | (default) Disable voltage low threshold
VoltageLow 0 | 2.0.6 | (default) Disable voltage low threshold
VoltageLow \<voltage\> | 2.0.6 | Set voltage low threshold value

### Domoticz
Command | Version | Description
------- | ------- | -----------
DomoticzIdx | 2.0.7 | Show current Domoticz relay1 index
DomoticzIdx off | 2.0.7 | (default) Disable use of Domoticz
DomoticzIdx 0 | 2.0.7 | (default) Disable use of Domoticz
DomoticzIdx \<idx\> | 2.0.7 | Set Domoticz relay1 index
DomoticzIdx\<index\> | 3.1.1 | Show current Domoticz relay1 to relay4 index
DomoticzIdx\<index\> off | 3.1.1 | (default) Disable use of Domoticz
DomoticzIdx\<index\> 0 | 3.1.1 | (default) Disable use of Domoticz
DomoticzIdx\<index\> \<idx\> | 3.1.1 | Set Domoticz relay1 to relay4 index
DomoticzInTopic | 2.0.7 | Show current Domoticz MQTT In Topic
DomoticzInTopic 1 | 2.0.7 | Reset Domoticz MQTT In Topic to ```user_config.h``` value and restart
DomoticzInTopic \<your-topic\> | 2.0.7 | Set Domoticz MQTT In Topic and restart
DomoticzKeyIdx | 2.0.7 | Show current Domoticz key1 index
DomoticzKeyIdx 0 | 2.0.7 | (default) Disable use of key1 index
DomoticzKeyIdx \<idx\> | 2.0.7 | Set Domoticz key1 index. To use it you'll need to enable ButtonTopic too
DomoticzKeyIdx\<index\> | 3.1.1 | Show current Domoticz key1 to key4 index
DomoticzKeyIdx\<index\> 0 | 3.1.1 | (default) Disable use of key1 to key4 index
DomoticzKeyIdx\<index\> \<idx\> | 3.1.1 | Set Domoticz key1 to key4 index. To use it you'll need to enable ButtonTopic too
DomoticzOutTopic | 2.0.7 | Show current Domoticz MQTT Out Topic
DomoticzOutTopic 1 | 2.0.7 | Reset Domoticz MQTT Out Topic to ```user_config.h``` value and restart
DomoticzOutTopic \<your-topic\> | 2.0.7 | Set Domoticz MQTT Out Topic and restart
DomoticzSensorIdx | 3.9.3 | Show current Domoticz sensor1 index
DomoticzSensorIdx 0 | 3.9.3 | (default) Disable use of sensor1 index
DomoticzSensorIdx \<idx\> | 3.9.3 | Set Domoticz sensor1 index.
DomoticzSensorIdx\<index\> | 3.9.3 | Show current Domoticz sensor1 to sensor5 index
DomoticzSensorIdx\<index\> 0 | 3.9.3 | (default) Disable use of sensor1 to sensor5 index
DomoticzSensorIdx\<index\> \<idx\> | 3.9.3 | Set Domoticz sensor1 to sensor5 index.
DomoticzSwitchIdx | 3.9.3 | Show current Domoticz switch1 index
DomoticzSwitchIdx 0 | 3.9.3 | (default) Disable use of switch1 index
DomoticzSwitchIdx \<idx\> | 3.9.3 | Set Domoticz switch1 index. To use it you'll need to enable SwitchTopic too
DomoticzSwitchIdx\<index\> | 3.9.3 | Show current Domoticz switch1 to switch4 index
DomoticzSwitchIdx\<index\> 0 | 3.9.3 | (default) Disable use of switch1 to switch4 index
DomoticzSwitchIdx\<index\> \<idx\> | 3.9.3 | Set Domoticz switch1 to switch4 index. To use it you'll need to enable SwitchTopic too
DomoticzUpdateTimer | 2.0.7 | Show current Domoticz update timer value in seconds
DomoticzUpdateTimer off | 2.0.7 | (default) Disable sending interrim Domoticz status
DomoticzUpdateTimer 0 | 2.0.7 | (default) Disable sending interrim Domoticz status
DomoticzUpdateTimer \<your-secs\> | 2.0.7 | Send status to Domoticz between every 1 and 3600 seconds

### WS2812 Led string
Command | Version | Description
------- | ------- | -----------
Color | 3.2.9 | Show current strip/ring color setting as RRGGBB
Color \<RRGGBB\> | 3.2.9 | Set strip/ring color to RRGGBB hexadecimal value
Dimmer | 3.2.9 | Show current dimmer setting from 0 to 100%
Dimmer 0 - 100 | 3.2.9 | Set dimmer value from 0 to 100%
Fade | 3.2.9 | Show current color fade state
Fade Off | 3.2.9 | (default) Do not use fade while changing colors
Fade On | 3.2.9 | Use fade while changing colors
Fade 0 | 3.2.9 | (default) Do not use fade while changing colors
Fade 1 | 3.2.9 | Use fade while changing colors
Led1 - Led\<pixelcount\> | 3.2.9 | Show specific led current color as RRGGBB
Led1 - Led\<pixelcount\> \<RRGGBB\> | 3.2.9 | Set specific led to desired color RRGGBB
LedTable | 3.2.9 | Show current Led table color correction state
LedTable Off | 3.2.9 | (default) Do not use Led table for color correction
LedTable On | 3.2.9 | Use Led table for color correction
LedTable 0 | 3.2.9 | (default) Do not use Led table for color correction
LedTable 1 | 3.2.9 | Use Led table for color correction
Pixels | 3.2.9 | Show current pixel count
Pixels \<count\> | 3.2.9 | Set amount of pixels in strip or ring up to 256
Scheme | 3.2.9 | Show current selected scheme
Scheme 0 | 3.2.9 | (default) Use single color for all leds in strip/ring
Scheme 1 | 3.2.10 | Start wakeup light
Scheme 2 | 3.2.10 | Show clock
Scheme 3 | 3.2.10 | Show incandescent pattern
Scheme 4 | 3.2.10 | Show rgb pattern
Scheme 5 | 3.2.10 | Show Christmas pattern
Scheme 6 | 3.2.10 | Show Hanukkah pattern
Scheme 7 | 3.2.10 | Show Kwanzaa pattern
Scheme 8 | 3.2.10 | Show rainbow pattern
Scheme 9 | 3.2.10 | Show fire pattern
Speed | 3.2.9 | Show current fade speed selection
Speed 1 - 5 | 3.2.9 | Select desired fade speed from 1 = fast to 5 = slow
Wakeup | 3.2.10 | Show current wake up light duration in seconds
Wakeup 1 - 3600 | 3.2.10 | Set wake up light duration in seconds
Width | 3.2.10 | Show current led group width
Width 0 - 4 | 3.2.10 | Set led group width used by Schemes 3 - 9

### Next Generation
Command | Version | Description
------- | ------- | -----------
Modules | 4.0.0 | Show available modules by name and index
Module | 4.0.0 | Show active module by name and index
Module \<index\> | 4.0.0 | Switch to selected module and restart
Gpios | 4.0.0 | Show available sensors and devices by name and index for user selection
Gpio | 4.0.0 | Show current GPIO usage for current module
Gpio\<pin\> \<sensor\> | 4.0.0 | Select sensor to be connected to \<pin\>

### Optional
Command | Version | Description
------- | ------- | -----------
I2Cscan | 2.0.20 | Scan I2C bus and show device addresses found
SwitchMode | 2.0.18 | Show current external switch mode
SwitchMode 0 | 2.0.18 | (default) Set switch mode to TOGGLE
SwitchMode 1 | 2.0.18 | Set switch mode to FOLLOW (0 = Off, 1 = On)
SwitchMode 2 | 2.0.18 | Set switch mode to inverted FOLLOW (0 = On, 1 = Off)
SwitchMode 3 | 3.0.2 | Set switch mode to PUSHBUTTON (Normally 1, 0 = toggle)
SwitchMode 4 | 3.0.2 | Set switch mode to inverted PUSHBUTTON (Normally 0, 1 = toggle)

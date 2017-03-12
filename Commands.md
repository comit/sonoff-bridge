The firmware supports a **serial**, **MQTT** and **Web** Man Machine interface. The serial interface is set to 115200 bps except for Sonoff Dual where it is set to 19200 bps. The MQTT commands are constructed from MQTT Topic for ```cmnd/sonoff/<command>``` and MQTT Message for ```<parameter>``` and are NOT case sensitive. 

Commands are allowed like ```cmnd/sonoff/<command><relay>``` to address a specific relay where applicable.

The commands can also be executed via HTTP requests.
```
http://sonoff/cm?cmnd=Power%20Off
http://sonoff/cm?cmnd=Power%20On
http://sonoff/cm?user=admin&password=joker&cmnd=Power%20Toggle
```

The following command tables are available:
- [Main](#main)
- [Management](#management)
- [Wifi](#wifi)
- [MQTT](#mqtt)
- [Logging](#logging)
- [Sonoff Pow specific](#sonoff-pow)
- [Sonoff Led specific](#sonoff-led)
- [Domoticz](#domoticz)
- [WS2812 led string](#ws2812-led-string)
- [IR remote control](#irremote)

### Main
```
Command          | Payload      | Description
-----------------|--------------|---------------------------------------------------------------------------
BlinkCount       |              | Show current BlinkCount
BlinkCount       | 0            | Blink many times before restoring power state
BlinkCount       | 1..32000     | Set how many blinks before restoring power state
BlinkTime        |              | Show current BlinkTime in 0.1 seconds
BlinkTime        | 2..3600      | Set BlinkTime with 0.1 seconds increment
LedPower         |              | Show current led power state as On or Off
LedPower         | 0 | off      | Turn led AND LedState Off
LedPower         | 1 | on       | Turn led On AND LedState Off
LedState         |              | Show current led state as 0 to 7
LedState         | 0 | off      | Disable use of LED as much as possible
LedState         | 1 | on       | Show power state on led
LedState         | 2            | Show MQTT subscriptions as a led blink
LedState         | 3            | Show power state and MQTT subscriptions as a led blink
LedState         | 4            | Show MQTT publications as a led blink
LedState         | 5            | Show power state and MQTT publications as a led blink
LedState         | 6            | Show all MQTT messages as a led blink
LedState         | 7            | Show power state and MQTT messages as a led blink
Light | Power<x> |              | Show current power state of relay<x> as On or Off
Light | Power<x> | 0 | off      | Turn power Off
Light | Power<x> | 1 | on       | Turn power On
Light | Power<x> | 2 | toggle   | Toggle power
Light | Power<x> | 3 | blink    | Blink power
Light | Power<x> | 4 | blinkoff | Stop blinking power
PowerOnState     |              | Show current relay power on state
PowerOnState     | 0 | off      | Keep relay(s) off after power on
PowerOnState     | 1 | on       | Turn relay(s) on after power on
PowerOnState     | 2 | toggle   | Toggle relay(s) on from last saved
PowerOnState     | 3            | (default) Turn relay(s) on as last saved
PowerRetain      |              | Show current MQTT power retain state
PowerRetain      | 0 | off      | (default) Disable MQTT power retain on status update
PowerRetain      | 1 | on       | Enable MQTT power retain on status update
PulseTime        |              | Show current PulseTime in 0.1 seconds
PulseTime        | 0 | off      | (Default) Disable use of PulseTime
PulseTime        | 1..111       | Set PulseTime with 0.1 seconds increment
PulseTime        | 112..64900   | Set PulseTime with 1 seconds increment starting with 12 seconds (113 = 13 seconds etc.)
SwitchMode<x>    |              | Show current external switch mode
SwitchMode<x>    | 0            | (default) Set switch mode to TOGGLE
SwitchMode<x>    | 1            | Set switch mode to FOLLOW (0 = Off, 1 = On)
SwitchMode<x>    | 2            | Set switch mode to inverted FOLLOW (0 = On, 1 = Off)
SwitchMode<x>    | 3            | Set switch mode to PUSHBUTTON (Normally 1, 0 = toggle)
SwitchMode<x>    | 4            | Set switch mode to inverted PUSHBUTTON (Normally 0, 1 = toggle)
```

### Management
```
Command         | Payload  | Description
----------------|----------|-------------------------------------------------------------------------------
ButtonRestrict  |          | Show current button multi press mode
ButtonRestrict  | 0 | Off  | (default) Allow all button actions
ButtonRestrict  | 1 | On   | Allow only single and double short press button actions
Emulation       |          | Show current emulation state
Emulation       | 0 | off  | Disable emulation
Emulation       | 1        | Enable Belkin WeMo emulation for Alexa
Emulation       | 2        | Enable Hue Bridge emulation for Alexa
FriendlyName    |          | Show friendly name as used by emulation
FriendlyName<x> |          | Show friendly name as used by emulation
FriendlyName<x> | 1        | Reset friendly name to user_config.h value (FRIENDLY_NAME)
FriendlyName<x> | <name>   | Set friendly name
Gpios           |          | Show available sensors and devices by name and index for user selection
Gpio            |          | Show current GPIO usage for current module
Gpio<pin>       | <sensor> | Select sensor to be connected to <pin>
I2Cscan         |          | Scan I2C bus and show device addresses found
Modules         |          | Show available modules by name and index
Module          |          | Show active module by name and index
Module          | <index>  | Switch to selected module and restart
Mqtt            |          | Show current MQTT state
Mqtt            | 0 | off  | Disable MQTT
Mqtt            | 1 | on   | Enable MQTT
OtaUrl          |          | Show current otaurl
OtaUrl          | 1        | Reset otaurl to user_config.h value (OTA_URL)
OtaUrl          | <url>    | Set otaurl
Reset           | 1        | Reset sonoff parameters to user_config.h values and restart
Reset           | 2        | Erase flash, reset sonoff parameters to user_config.h values and restart
Restart         | 1        | Restart sonoff
Restart         | 99       | Force restart sonoff without config save
SaveData        |          | Save parameter changes and show current state as Manual, On or every x seconds
SaveData        | 0 | off  | Save parameter changes only manually
SaveData        | 1 | on   | (default) Save parameter changes every second
SaveData        | <sec>    | Save parameter changes between every 2 and 3600 seconds
SaveState       |          | Show current SaveState state
SaveState       | 1 | on   | (default) Save power changes and set relay after restart
SaveState       | 0 | off  | Do not save power changes and do not set relay after restart
Sleep           |          | Show current sleep state as 0 (Off) or duration of up to 250 mSec
Sleep           | 0 | off  | (default) Turn sleep off
Sleep           | 1..250   | Set sleep duration from 1 to 250 mSec to enable energy saving
Status          |          | Show abbreviated status information
Status          | 0        | Show all status information
Status          | 1        | Show more status information
Status          | 2        | Show firmware information
Status          | 3        | Show syslog information
Status          | 4        | Show memory information
Status          | 5        | Show network information
Status          | 6        | Show MQTT information
Status          | 7        | Show Real Time Clock information
Status          | 8        | (Sonoff Pow only) Show Power usage
Status          | 9        | (Sonoff Pow only) Show Power thresholds
Status          | 10       | Show sensor information
Timezone        |          | Show current timezone
Timezone        | -12..12  | Set timezone
Timezone        | 99       | Use Daylight Saving parameters from user_config.h (TIME_DST and TIME_STD)
Upgrade         | 1        | Download ota firmware from your web server and restart
Upload          | 1        | Download ota firmware from your web server and restart
```

### Wifi
```
Command        | Payload   | Description
---------------|-----------|-------------------------------------------------------------------------------
AP             |           | Show current selected Wifi Access Point (AP)
AP             | 0         | Switch to other Wifi Access Point (AP)
AP             | 1         | Select Wifi Access Point 1 (AP)
AP             | 2         | Select Wifi Access Point 2 (AP)
Hostname       |           | Show current hostname
Hostname       | 1         | Reset hostname to MQTT_TOPIC-<4digits> and restart
Hostname       | <host>    | Set hostname and restart
NtpServer<x>   |           | Show NTP server 1 to 3 name or ip address
NtpServer<x>   | <ntphost  | Set NTP server 1 to 3 name or ip address
Password       |           | Show AP1 current Wifi password
Password       | 1         | Reset AP1 Wifi password to user_config.h (STA_PASS1) and restart
Password       | <passwrd> | Set AP1 Wifi password and restart
Password<x>    |           | Show APx current Wifi password
Password<x>    | 1         | Reset APx Wifi password to user_config.h (STA_PASS1 or STA_PASS2) and restart
Password<x>    | <passwrd> | Set APx Wifi password and restart
SSId | SSId<x> |           | Show APx current Wifi SSId
SSId | SSId<x> | 1         | Reset APx Wifi SSId to user_config.h (STA_SSID1 or STA_SSID2) and restart
SSId | SSId<x> | <ssid>    | Set APx Wifi SSId and restart
WebPassword    |           | Show current web server Admin password for user WEB_USERNAME
WebPassword    | 0 | Off   | Disable use of password
WebPassword    | 1         | Reset password to value in user_config.h (WEB_PASSWORD)
WebPassword    | <passwrd> | Set web server Admin password for user WEB_USERNAME
WebServer      |           | Show current web server state
WebServer      | 0 | Off   | Stop web server
WebServer      | 1 | User  | Start web server in user mode
WebServer      | 2 | Admin | Start web server in admin mode
WifiConfig     |           | Show current config tool
WifiConfig     | 0         | Disable wifi config but restart (used with alternate AP)
WifiConfig     | 1         | Start smart config for 1 minute and set as current config tool
WifiConfig     | 2         | Start wifi manager (web server at 192.168.4.1) and set as current config tool
WifiConfig     | 3         | Start WPS config for 1 minute and set as current config tool
WifiConfig     | 4         | Disable wifi config but retry other AP without restart
```

### MQTT
```
Command      | Payload      | Description
-------------|--------------|------------------------------------------------------------------------------
ButtonRetain |              | Show current button MQTT retain flag state
ButtonRetain | 0 | off      | (default) Disable use of MQTT retain flag
ButtonRetain | 1 | on       | Set ButtonTopic to Topic and enable MQTT retain flag on button press
ButtonTopic  |              | Show current MQTT button topic
ButtonTopic  | 0            | Disable use of MQTT button topic
ButtonTopic  | 1            | Set MQTT button topic to Topic
ButtonTopic  | <topic>      | Set MQTT button topic
GroupTopic   |              | Show current MQTT group topic
GroupTopic   | 1            | Reset MQTT group topic to user_config.h (MQTT_GRPTOPIC) and restart
GroupTopic   | <grouptopic> | Set MQTT group topic and restart
MqttClient   |              | Show current MQTT client
MqttClient   | 1            | Reset MQTT client to user_config.h (MQTT_CLIENT_ID) and restart
MqttClient   | <client>     | Set MQTT client and restart. May use wildcard %06X to be replaced by last six characters of MAC address
MqttHost     |              | Show current MQTT host
MqttHost     | 1            | Reset MQTT host to user_config.h (MQTT_HOST) and restart
MqttHost     | <host>       | Set MQTT host and restart
MqttPassword |              | Show current MQTT password
MqttPassword | 1            | Reset MQTT password to user_config.h (MQTT_PASS) and restart
MqttPassword | <pswrd>      | Set MQTT password and restart
MqttPort     |              | Show current MQTT port
MqttPort     | 1            | Reset MQTT port to user_config.h (MQTT_PORT) and restart
MqttPort     | <port>       | Set MQTT port between 2 and 32766 and restart
MqttUser     |              | Show current MQTT user name
MqttUser     | 1            | Reset MQTT user name to user_config.h (MQTT_USER) and restart
MqttUser     | <user>       | Set MQTT user name and restart
Units        |              | Show current Units state
Units        | 0 | off      | (default) Do not show units to messages
Units        | 1 | on       | Add units to messages
PowerRetain  |              | Show current MQTT power retain state
PowerRetain  | 0 | off      | (default) Disable MQTT power retain on status update
PowerRetain  | 1 |on        | Enable MQTT power retain on status update
SwitchRetain |              | Show current button MQTT retain flag state
SwitchRetain | 0 | off      | (default) Disable use of MQTT retain flag
SwitchRetain | 1 | on       | Set ButtonTopic to Topic and enable MQTT retain flag on button press
SwitchTopic  |              | Show current MQTT switch topic
SwitchTopic  | 0            | Disable use of MQTT switch topic
SwitchTopic  | 1            | Set MQTT switch topic to Topic
SwitchTopic  | <topic>      | Set MQTT switch topic
TelePeriod   |              | Show current telemetry period in seconds
TelePeriod   | 0 | off      | Disable telemetry messages
TelePeriod   | 1            | Reset telemetry period to user_config.h (TELE_PERIOD)
TelePeriod   | <secs>       | Set telemetry period between 2 and 3600 seconds
Topic        |              | Show current MQTT topic
Topic        | 1            | Reset MQTT topic to user_config.h (MQTT_TOPIC) and restart
Topic        | <topic>      | Set MQTT topic  AND button topic and restart
```

### Logging
```
Command         | Payload | Description
----------------|---------|--------------------------------------------------------
LogHost         |         | Show current syslog host
LogHost         | 1       | Reset syslog host to user_config.h (SYS_LOG_HOST)
LogHost         | <host>  | Set syslog host
LogPort         |         | Show current syslog port
LogPort         | 1       | Reset syslog port to user_config.h (SYS_LOG_PORT)
LogPort         | <port>  | Set syslog port between 2 and 32766
SerialLog       |         | Show current serial log level
SerialLog       | 0 | off | Disable serial logging
SerialLog       | 1       | Show only error messages
SerialLog       | 2       | Show error and info messages
SerialLog       | 3       | Show error, info and debug messages
SerialLog       | 4       | Show all messages
SysLog          |         | Show current syslog level
SysLog          | 0 | off | Disable syslog logging
SysLog          | 1       | Show only error messages
SysLog          | 2       | Show error and info messages
SysLog          | 3       | Show error, info and debug messages
SysLog          | 4       | Show all messages
WebLog          |         | Show current serial log level
WebLog          | 0 | off | Disable serial logging
WebLog          | 1       | Show only error messages
WebLog          | 2       | Show error and info messages
WebLog          | 3       | Show error, info and debug messages
WebLog          | 4       | Show all messages
```

### Sonoff Pow
```
Command         | Payload     | Description
----------------|-------------|----------------------------------------------------
CurrentHigh     |             | Show current current high threshold value
CurrentHigh     | 0 | off     | (default) Disable current high threshold
CurrentHigh     | <milliamps> | Set current high threshold value
CurrentLow      |             | Show current current low threshold value
CurrentLow      | 0 | off     | (default) Disable current low threshold
CurrentLow      | <milliamps> | Set current low threshold value
HlwIcal         |             | Show calibration value for Current
HlwIcal         | 1           | Set program calibration value for Current
HlwIcal         | <value>     | Set user calibration value for Current
HlwPcal         |             | Show calibration value for Power
HlwPcal         | 1           | Set program calibration value for Power
HlwIcal         | <value>     | Set user calibration value for Power
HlwUcal         |             | Show calibration value for Voltage
HlwUcal         | 1           | Set program calibration value for Voltage
HlwIcal         | <value>     | Set user calibration value for Voltage
PowerHigh       |             | Show current power high threshold value
PowerHigh       | 0 | off     | (default) Disable power high threshold
PowerHigh       | <watt>      | Set power high threshold value
PowerLow        |             | Show current power low threshold value
PowerLow        | 0 | off     | (default) Disable power low threshold
PowerLow        | <watt>      | Set power low threshold value
Status          | 8           | Show Power usage
Status          | 9           | Show Power thresholds
VoltageHigh     |             | Show current voltage high threshold value
VoltageHigh     | 0 | off     | (default) Disable voltage high threshold
VoltageHigh     | <voltage>   | Set voltage high threshold value
VoltageLow      |             | Show current voltage low threshold value
VoltageLow      | 0 | off     | (default) Disable voltage low threshold
VoltageLow      | <voltage>   | Set voltage low threshold value
```

### Sonoff Led
```
Command         | Payload | Description
----------------|---------|--------------------------------------------------------
Color           |         | Show current color setting as CCWW
Color           | <CCWW>  | Set color to CCWW hexadecimal value
Dimmer          |         | Show current dimmer setting from 0 to 100%
Dimmer          | 0..100  | Set dimmer value from 0 to 100%
Fade            |         | Show current fade state
Fade            | 0 | Off | (default) Do not use fade
Fade            | 1 | on  | Use fade
LedTable        |         | Show current Led table intensity correction state
LedTable        | 0 | off | (default) Do not use Led table for intensity correction
LedTable        | 1 | on  | Use Led table for intensity correction
Speed           |         | Show current fade speed selection
Speed           | 1..8    | Select desired fade speed from 1 = fast to 8 = slow
Wakeup          |         | Start wake up sequence from Off to Dimmer value
WakeupDuration  |         | Show current wake up light duration in seconds
WakeupDuration  | 1..3600 | Set wake up light duration in seconds
```

### Domoticz
```
Command              | Payload | Description
---------------------|---------|---------------------------------------------------------------------------
DomoticzIdx<x>       |         | Show current Domoticz relay1 to relay4 index
DomoticzIdx<x>       | 0 | off | (default) Disable use of Domoticz
DomoticzIdx<x>       | <idx>   | Set Domoticz relay1 to relay4 index
DomoticzInTopic      |         | Show current Domoticz MQTT In Topic
DomoticzInTopic      | 1       | Reset Domoticz MQTT In Topic to user_config.h (DOMOTICZ_IN_TOPIC) and restart
DomoticzInTopic      | <topic> | Set Domoticz MQTT In Topic and restart
DomoticzKeyIdx<x>    |         | Show current Domoticz key1 to key4 index
DomoticzKeyIdx<x>    | 0       | (default) Disable use of key1 to key4 index
DomoticzKeyIdx<x>    | <idx>   | Set Domoticz key1 to key4 index. To use it you'll need to enable ButtonTopic too
DomoticzOutTopic     |         | Show current Domoticz MQTT Out Topic
DomoticzOutTopic     | 1       | Reset Domoticz MQTT Out Topic to user_config.h (DOMOTICZ_OUT_TOPIC) and restart
DomoticzOutTopic     | <topic> | Set Domoticz MQTT Out Topic and restart
DomoticzSensorIdx<x> |         | Show current Domoticz sensor1 to sensor5 index
DomoticzSensorIdx<x> | 0       | (default) Disable use of sensor1 to sensor5 index
DomoticzSensorIdx<x> | <idx>   | Set Domoticz sensor1 to sensor5 index.
DomoticzSwitchIdx<x> |         | Show current Domoticz switch1 to switch4 index
DomoticzSwitchIdx<x> | 0       | (default) Disable use of switch1 to switch4 index
DomoticzSwitchIdx<x> | <idx>   | Set Domoticz switch1 to switch4 index. To use it you'll need to enable SwitchTopic too
DomoticzUpdateTimer  |         | Show current Domoticz update timer value in seconds
DomoticzUpdateTimer  | 0 | off | (default) Disable sending interrim Domoticz status
DomoticzUpdateTimer  | 1..3600 | Send status to Domoticz between every 1 and 3600 seconds
```

### WS2812 Led string
```
Command          | Payload  | Description
-----------------|----------|------------------------------------------------------
Color            |          | Show current strip/ring color setting as RRGGBB
Color            | <RRGGBB> | Set strip/ring color to RRGGBB hexadecimal value
Dimmer           |          | Show current dimmer setting from 0 to 100%
Dimmer           | 0..100   | Set dimmer value from 0 to 100%
Fade             |          | Show current color fade state
Fade             | 0 | Off  | (default) Do not use fade while changing colors
Fade             | 1 | on   | Use fade while changing colors
Led1..Led<count> |          | Show specific led current color as RRGGBB
Led1..Led<count> | <RRGGBB> | Set specific led to desired color RRGGBB
LedTable         |          | Show current Led table color correction state
LedTable         | 0 | off  | (default) Do not use Led table for color correction
LedTable         | 1 | on   | Use Led table for color correction
Pixels           |          | Show current pixel count
Pixels           | <count>  | Set amount of pixels in strip or ring up to 256
Scheme           |          | Show current selected scheme
Scheme           | 0        | (default) Use single color for all leds in strip/ring
Scheme           | 1        | Start wakeup light
Scheme           | 2        | Show clock
Scheme           | 3        | Show incandescent pattern
Scheme           | 4        | Show rgb pattern
Scheme           | 5        | Show Christmas pattern
Scheme           | 6        | Show Hanukkah pattern
Scheme           | 7        | Show Kwanzaa pattern
Scheme           | 8        | Show rainbow pattern
Scheme           | 9        | Show fire pattern
Speed            |          | Show current fade speed selection
Speed            | 1..5     | Select desired fade speed from 1 = fast to 5 = slow
Wakeup           |          | Show current wake up light duration in seconds
Wakeup           | 1..3600  | Set wake up light duration in seconds
Width            |          | Show current led group width
Width            | 0..4     | Set led group width used by Schemes 3 - 9
```

###IRremote
```
Command   | Payload                 | Description
----------|-------------------------|------------------------------------------------------
IRsend    | {"protocol": "<proto>", | Send IR remote control as JSON encapsulated command.
          |  "bits": 1..32          | <proto> is NEC, SONY, RC5, RC6, DISH, JVC or SAMSUNG
          |  "data": 1..(2^32)-1}   | bits are the required number of data bits.
          |                         | data is the data frame as 32 bit unsigned integer.
          |                         | See http://www.lirc.org/ for more info.
```
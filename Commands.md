
The Sonoff-Tasmota firmware provides three powerful man machine interfaces: **MQTT**, **web** and **serial**.

### MQTT

MQTT is the recommended interaction interface. You can find all relevant details regarding MQTT in the [MQTT Essentials](http://www.hivemq.com/mqtt-essentials/) article series. You'll need an MQTT broker in place and should utilize an independent [MQTT client](http://www.hivemq.com/blog/seven-best-mqtt-client-tools) for troubleshooting. Setting up the basic MQTT environment is out of the scope of this article.

Please check the specific [MQTT Features](https://github.com/arendst/Sonoff-Tasmota/wiki/MQTT-Features) wiki page to learn more.

**Example:**

A Sonoff-Tasmota module was configured with the *FullTopic* `tasmota/%topic%/%prefix%/` and the topic setting "sonoff-mylight". We want to switch the light On and Off.

By looking at the commands table below we can learn about the [Power](https://github.com/arendst/Sonoff-Tasmota/wiki/Commands#main) command and the TOGGLE option. "Power1" represents the first relay.

* Status query:
  ```java
  tasmota/sonoff-mylight/cmnd/Power1 ←      //empty message
     ↳ tasmota/sonoff-mylight/stat/RESULT → {"POWER1":"OFF"}
     ↳ tasmota/sonoff-mylight/stat/POWER1 → OFF
  ```
  We can see, that the module's first relay is currently turned off.

* Sending a command:
  ```java
  tasmota/sonoff-mylight/cmnd/Power1 ← "TOGGLE"
     ↳ // Power for relay 1 is toggled
     ↳ tasmota/sonoff-mylight/stat/RESULT → {"POWER1":"ON"}
     ↳ tasmota/sonoff-mylight/stat/POWER1 → ON
  ```
  We've send the toggle command and received the new state confirmation.

**Side Note:** For many commands, an empty payload is a *query*. If you are using *mosquitto_pub*, you can issue an empty payload using the `-n` command line option. If your MQTT client cannot issue an empty payload, you can use the single character "?" instead.

### Web 

Commands can be executed via HTTP requests, for example:

```http
http://sonoff/cm?cmnd=Power%20TOGGLE
http://sonoff/cm?cmnd=Power%20On
http://sonoff/cm?cmnd=Power%20off
http://sonoff/cm?user=admin&password=joker&cmnd=Power%20Toggle
```

### Serial

The serial interface is set to 115200 bps except for Sonoff Dual where it is set to 19200 bps.

## Using backlog
Starting with version 5.3.0 a backlog feature is available allowing to execute several commands in sequence (maximum 16/request). Examples of this command are:
```
Backlog status 1; power 2; delay 20; power 2; status 4
Backlog ssid1 myssid; password1 mypassword
```

## Command Overview

The following command tables are available:

- [Main](#main)
- [Sensor](#sensor)
- [Management](#management)
- [Wifi](#wifi)
- [MQTT](#mqtt-specific)
- [SetOption Overview](#setoption-overview)
- [Logging](#logging)
- [Sonoff Pow specific](#sonoff-pow)
- [Sonoff Led, B1 and BN-SZ01 specific](#sonoff-led-b1-and-bn-sz01)
- [Sonoff RF Bridge 433](#sonoff-rf-bridge-433)
- [Domoticz](#domoticz)
- [WS2812 led string](#ws2812-led-string)
- [IR remote control](#irremote)

### Main

Command          | Payload      | Description
-----------------|--------------|---------------------------------------------------------------------------
BlinkCount       |              | Show current BlinkCount
BlinkCount       | 0            | Blink many times before restoring power state
BlinkCount       | 1..32000     | Set how many blinks before restoring power state
BlinkTime        |              | Show current BlinkTime in 0.1 seconds
BlinkTime        | 2..3600      | Set BlinkTime with 0.1 seconds increment
LedPower         |              | Show current led power state as On or Off
LedPower         | 0 / off      | Turn led AND LedState Off
LedPower         | 1 / on       | Turn led On AND LedState Off
LedState         |              | Show current led state as 0 to 7
LedState         | 0 / off      | Disable use of LED as much as possible
LedState         | 1 / on       | Show power state on led (inverted for Sonoff Touch)
LedState         | 2            | Show MQTT subscriptions as a led blink
LedState         | 3            | Show power state and MQTT subscriptions as a led blink
LedState         | 4            | Show MQTT publications as a led blink
LedState         | 5            | Show power state and MQTT publications as a led blink
LedState         | 6            | Show all MQTT messages as a led blink
LedState         | 7            | Show power state and MQTT messages as a led blink
Power\<x\>       |              | Show current power state of relay\<x\> as On or Off
Power\<x\>       | 0 / off      | Turn relay\<x\> power Off
Power\<x\>       | 1 / on       | Turn relay\<x\> power On
Power\<x\>       | 2 / toggle   | Toggle power of relay\<x\>
Power\<x\>       | 3 / blink    | Blink power of relay\<x\>
Power\<x\>       | 4 / blinkoff | Stop blinking power of relay\<x\>
PowerOnState     |              | Show current relay power on state
PowerOnState     | 0 / off      | Keep relay(s) off after power on
PowerOnState     | 1 / on       | Turn relay(s) on after power on
PowerOnState     | 2 / toggle   | Toggle relay(s) on from last saved
PowerOnState     | 3            | (default) Turn relay(s) on as last saved
PowerOnState     | 4            | Turn relay(s) on and disable further relay control
PowerRetain      |              | Show current MQTT power retain state
PowerRetain      | 0 / off      | (default) Disable MQTT power retain on status update
PowerRetain      | 1 / on       | Enable MQTT power retain on status update
PulseTime\<x\>   |              | Show current PulseTime of relay\<x\> in 0.1 seconds
PulseTime\<x\>   | 0 / off      | (Default) Disable use of PulseTime for relay\<x\>
PulseTime\<x\>   | 1..111       | Set PulseTime for relay\<x\> with 0.1 seconds increment
PulseTime\<x\>   | 112..64900   | Set PulseTime for relay\<x\> with 1 seconds increment starting with 12 seconds (113 = 13 seconds etc.)
SetOption11      |              | Show current button single and double press swap state
SetOption11      | 0 / off      | (default) Legacy button single and double press
SetOption11      | 1 / on       | Button single and double press functionality swapped
SetOption13      |              | Show current button single press only state
SetOption13      | 0 / off      | (default) Enable button single, multi-press and hold
SetOption13      | 1 / on       | Enable button single press only for immediate response. Disable by holding 4 x SetOption32
SetOption14      |              | Show current relay switch mode
SetOption14      | 0 / off      | (default) Set self-locking mode for all relays
SetOption14      | 1 / on       | Set interlock mode for all relays 
SetOption32      |              | Show current key hold time in 0.1 seconds
SetOption32      | 1..100       | Set key hold time from 0.1 to 10 seconds
SwitchMode\<x\>  |              | Show current external switch mode
SwitchMode\<x\>  | 0            | (default) Set switch mode to TOGGLE
SwitchMode\<x\>  | 1            | Set switch mode to FOLLOW (0 = Off, 1 = On)
SwitchMode\<x\>  | 2            | Set switch mode to inverted FOLLOW (0 = On, 1 = Off)
SwitchMode\<x\>  | 3            | Set switch mode to PUSHBUTTON (Normally 1, 0 = toggle)
SwitchMode\<x\>  | 4            | Set switch mode to inverted PUSHBUTTON (Normally 0, 1 = toggle)
SwitchMode\<x\>  | 5            | Set switch mode to PUSHBUTTON (Normally 1, 0 = toggle, Hold = hold)
SwitchMode\<x\>  | 6            | Set switch mode to inverted PUSHBUTTON (Normally 0, 1 = toggle, Hold = hold)

### Sensor

Command           | Payload        | Description
------------------|----------------|---------------------------------------------------------------
Counter\<x\>      |                | Show current Counter 1..4 value
Counter\<x\>      | 0              | Reset Counter 1..4
Counter\<x\>      | 1..64900       | Preset Counter 1..4
CounterDebounce   |                | Show current global Counter debounce time in mSec
CounterDebounce   | 0 / off        | Turn global Counter debounce off
CounterDebounce   | 1..3200        | Set global Counter debounce in mSec
CounterType\<x\>  |                | Show current Counter 1..4 type as pulse Counter or pulse Timer
CounterType\<x\>  | 0 / off        | Set current Counter 1..4 type as pulse Counter
CounterType\<x\>  | 1 / on         | Set current Counter 1..4 type as pulse Timer
EnergyRes         |                | Show current Energy Resolution
EnergyRes         | 0..5           | Set Energy Resolution
HumRes            |                | Show current Humidity Resolution
HumRes            | 0..3           | Set Humidity Resolution
PressRes          |                | Show current Pressure Resolution
PressRes          | 0..3           | Set Pressure Resolution
SetOption8        |                | Show current Temperature as either Celsius or Fahrenheit
SetOption8        | 0 / celsius    | Set Temperature to Celsius
SetOption8        | 1 / fahrenheit | Set Temperature to Fahrenheit
TempRes           |                | Show current Temperature Resolution
TempRes           | 0..3           | Set Temperature Resolution
TempUnit          |                | Replaced by SetOption8

### Management

Command           | Payload    | Description
------------------|------------|-------------------------------------------------------------------------------
Backlog           |            | Abort backlog if active
Backlog           | \<cmnds\>  | List of commands to be executed in sequence separated by a ;
ButtonRestrict    |            | Replaced by SetOption1
Delay             |            | Reset backlog delay to 0.2 seconds
Delay             | 2..3600    | Set delay between two backlog commands with 0.1 seconds increment
Emulation         |            | Show current emulation state
Emulation         | 0 / off    | Disable emulation
Emulation         | 1          | Enable Belkin WeMo emulation for Alexa
Emulation         | 2          | Enable Hue Bridge emulation for Alexa
FriendlyName      |            | Show friendly name as used by emulation
FriendlyName\<x\> |            | Show friendly name as used by emulation
FriendlyName\<x\> | 1          | Reset friendly name to `user_config.h` value (`FRIENDLY_NAME`)
FriendlyName\<x\> | \<name\>   | Set friendly name (32 chars max)
Gpios             |            | Show available sensors and devices by name and index for user selection
Gpio              |            | Show current GPIO usage for current module
Gpio\<pin\>       | \<sensor\> | Select sensor to be connected to \<pin\>
I2Cscan           |            | Scan I2C bus and show device addresses found
Modules           |            | Show available modules by name and index
Module            |            | Show active module by name and index
Module            | \<index\>  | Switch to selected module and restart
Mqtt              |            | Replaced by SetOption3
OtaUrl            |            | Show current otaurl
OtaUrl            | 1          | Reset otaurl to `user_config.h` value (`OTA_URL`)
OtaUrl            | \<url\>    | Set otaurl (100 chars max)
Pwm               |            | Show current PWM setting for enabled channels
Pwm\<x\>          | 0..1023    | Set Pwm channel x to value ranging from 0 to 1023
Reset             | 1          | Reset sonoff parameters to `user_config.h` values and restart
Reset             | 2          | Erase flash, reset sonoff parameters to `user_config.h` values and restart
Restart           | 1          | Restart sonoff
Restart           | 99         | Force restart sonoff without config save
SaveData          |            | Save parameter changes and show current state as Manual, On or every x seconds
SaveData          | 0 / off    | Save parameter changes only manually
SaveData          | 1 / on     | (default) Save parameter changes every second
SaveData          | \<sec\>    | Save parameter changes between every 2 and 3600 seconds
SaveState         |            | Replaced by SetOption0
SetOption0        |            | Show current Save power changes state
SetOption0        | 1 / on     | (default) Save power changes and set relay after restart
SetOption0        | 0 / off    | Do not save power changes and do not set relay after restart
SetOption1        |            | Show current button multi press mode
SetOption1        | 0 / off    | (default) Allow all button actions
SetOption1        | 1 / on     | Allow only single, double and hold press button actions
SetOption12       |            | Show current configuration flash usage option
SetOption12       | 0 / off    | (default) Use dynamic flash to save configuration lowering flash wear
SetOption12       | 1 / on     | Legacy save configuration in eeprom flash location only
Sleep             |            | Show current sleep state as 0 (Off) or duration of up to 250 mSec
Sleep             | 0 / off    | (default) Turn sleep off
Sleep             | 1..250     | Set sleep duration from 1 to 250 mSec to enable energy saving
Status            |            | Show abbreviated status information
Status            | 0          | Show all status information
Status            | 1          | Show more status information
Status            | 2          | Show firmware information
Status            | 3          | Show syslog information
Status            | 4          | Show memory information
Status            | 5          | Show network information
Status            | 6          | Show MQTT information
Status            | 7          | Show Real Time Clock information
Status            | 8          | (Sonoff Pow only) Show Power usage
Status            | 9          | (Sonoff Pow only) Show Power thresholds
Status            | 10         | Show sensor information
Status            | 11         | Show power information equal to teleperiod state message
Timezone          |            | Show current timezone
Timezone          | -12..12    | Set timezone
Timezone          | 99         | Use Daylight Saving parameters from `user_config.h` (`TIME_DST` and `TIME_STD`)
Upgrade           | 1          | Download ota firmware from your web server and restart
Upgrade           | \<version\> | Download ota firmware from web server if \<version\> is higher than device version
Upload            | 1          | Download ota firmware from your web server and restart
Upload            | \<version\> | Download ota firmware from web server if \<version\> is higher than device version


### Wifi

Command          | Payload     | Description
-----------------|-------------|-------------------------------------------------------------------------------
AP               |             | Show current selected Wifi Access Point (AP)
AP               | 0           | Switch to other Wifi Access Point (AP)
AP               | 1           | Select Wifi Access Point 1 (AP)
AP               | 2           | Select Wifi Access Point 2 (AP)
Hostname         |             | Show current hostname
Hostname         | 1           | Reset hostname to `MQTT_TOPIC-<4digits>` and restart
Hostname         | \<host\>    | Set hostname (32 chars max) and restart. If \<host\> contains % the hostname will be reset to the default.
IPAddress1       |             | Show current IP address
IPAddress1       | 0.0.0.0     | Use dynamic IP addresses (DHCP) and restart
IPAddress1       | x.x.x.x     | Set (decimal) static IP address and restart
IPAddress2       |             | Show current IP address of Gateway
IPAddress2       | x.x.x.x     | Set (decimal) IP address of Gateway and restart
IPAddress3       |             | Show current subnet mask
IPAddress3       | x.x.x.x     | Set (decimal) subnet mask and restart
IPAddress4       |             | Show current IP address of DNS server
IPAddress4       | x.x.x.x     | Set (decimal) IP address of DNS server and restart
NtpServer\<x\>   |             | Show NTP server 1 to 3 name or ip address
NtpServer\<x\>   | 0           | Set NTP server 1 to 3 name to none and restart
NtpServer\<x\>   | 1           | Reset NTP server 1 to 3 name to `user_config.h` (`NTP_SERVERx`) and restart
NtpServer\<x\>   | \<ntphost\> | Set NTP server 1 to 3 name or ip address (32 chars max)
Password         |             | Show AP1 current Wifi password
Password         | 1           | Reset AP1 Wifi password to `user_config.h` (`STA_PASS1`) and restart
Password         | \<pwd\>     | Set AP1 Wifi password (64 chars max) and restart
Password\<x\>    |             | Show APx current Wifi password
Password\<x\>    | 1           | Reset APx Wifi password to `user_config.h` (`STA_PASS1` or `STA_PASS2`) and restart
Password\<x\>    | \<pwd\>     | Set APx Wifi password (64 chars max) and restart
SSId / SSId\<x\> |             | Show APx current Wifi SSId
SSId / SSId\<x\> | 1           | Reset APx Wifi SSId to `user_config.h` (`STA_SSID1` or `STA_SSID2`) and restart
SSId / SSId\<x\> | \<ssid\>    | Set APx Wifi SSId (32 chars max) and restart
WebPassword      |             | Show current web server Admin password for user `WEB_USERNAME`
WebPassword      | 0           | Disable use of password
WebPassword      | 1           | Reset password to value in `user_config.h` (`WEB_PASSWORD`)
WebPassword      | \<pwd\>     | Set web server Admin password for user `WEB_USERNAME` (32 chars max)
WebServer        |             | Show current web server state
WebServer        | 0 / off     | Stop web server
WebServer        | 1 / user    | Start web server in user mode
WebServer        | 2 / admin   | Start web server in admin mode
WifiConfig       |             | Show current config tool
WifiConfig       | 0           | Disable wifi config but restart (used with alternate AP)
WifiConfig       | 1           | Start smart config for 1 minute and set as current config tool
WifiConfig       | 2           | Start wifi manager (web server at 192.168.4.1) and set as current config tool
WifiConfig       | 3           | Start WPS config for 1 minute and set as current config tool
WifiConfig       | 4           | Disable wifi config but retry other AP without restart


### MQTT Specific

Command         | Payload        | Description
----------------|----------------|-----------------------------------------------------------------------------
ButtonRetain    |                | Show current button MQTT retain flag state
ButtonRetain    | 0 / off        | (default) Disable use of MQTT retain flag
ButtonRetain    | 1 / on         | Set ButtonTopic to Topic and enable MQTT retain flag on button press
ButtonTopic     |                | Show current MQTT button topic
ButtonTopic     | 0 / off        | Disable use of MQTT button topic
ButtonTopic     | 1              | Set MQTT button topic to Topic
ButtonTopic     | \<topic\>      | Set MQTT button topic (32 chars max)
FullTopic       |                | Show current MQTT fulltopic string
FullTopic       | 1              | Reset MQTT fulltopic to `user_config.h` (`MQTT_FULLTOPIC`) and restart
FullTopic       | \<fulltopic\>  | Set MQTT fulltopic (100 chars max) using optional %topic% and %prefix% and restart
GroupTopic      |                | Show current MQTT group topic
GroupTopic      | 1              | Reset MQTT group topic to `user_config.h` (`MQTT_GRPTOPIC`) and restart
GroupTopic      | \<grouptopic\> | Set MQTT group topic (32 chars max) and restart
MqttClient      |                | Show current MQTT client
MqttClient      | 1              | Reset MQTT client to `user_config.h` (`MQTT_CLIENT_ID`) and restart
MqttClient      | \<client\>     | Set MQTT client (32 chars max) and restart. May use wildcard %06X to be replaced by last six characters of MAC address
MqttFingerprint |                | (  TLS only) Show current fingerprint
MqttFingerprint | \<print\>      |   (TLS only) Set current fingerprint as 20 space separated bytes (59 chars max)
MqttHost        |                | Show current MQTT host
MqttHost        | 1              | Reset MQTT host to `user_config.h` (`MQTT_HOST`) and restart
MqttHost        | \<host\>       | Set MQTT host (32 chars max) and restart
MqttPassword    |                | Show current MQTT password
MqttPassword    | 0              | Set MQTT password to none
MqttPassword    | 1              | Reset MQTT password to `user_config.h` (`MQTT_PASS`) and restart
MqttPassword    | \<pswrd\>      | Set MQTT password (32 chars max) and restart
MqttPort        |                | Show current MQTT port
MqttPort        | 1              | Reset MQTT port to `user_config.h` (`MQTT_PORT`) and restart
MqttPort        | \<port\>       | Set MQTT port between 2 and 32766 and restart
MqttResponse    |                | Replaced by SetOption4
MqttRetry       |                | Show current MQTT connection retry timer in seconds
MqttRetry       | 10             | (default) Set MQTT connection retry timer in seconds
MqttRetry       | 10..32000      | Set MQTT connection retry timer in seconds
MqttUser        |                | Show current MQTT user name
MqttUser        | 0              | Set MQTT user name to none
MqttUser        | 1              | Reset MQTT user name to `user_config.h` (`MQTT_USER`) and restart
MqttUser        | \<user\>       | Set MQTT user name (32 chars max) and restart
PowerRetain     |                | Show current MQTT power retain state
PowerRetain     | 0 / off        | (default) Disable MQTT power retain on status update
PowerRetain     | 1 / on         | Enable MQTT power retain on status update
Prefix1         |                | Show current MQTT command subscription prefix
Prefix1         | 1              | Reset MQTT command subscription prefix to `user_config.h` (`SUB_PREFIX`) and restart
Prefix1         | \<prefix\>     | Set MQTT command subscription prefix (10 chars max) and restart
Prefix2         |                | Show current MQTT status prefix
Prefix2         | 1              | Reset MQTT status prefix to `user_config.h` (`PUB_PREFIX`) and restart
Prefix2         | \<prefix\>     | Set MQTT status prefix (10 chars max) and restart
Prefix3         |                | Show current MQTT telemetry prefix
Prefix3         | 1              | Reset MQTT telemetry prefix to `user_config.h` (`PUB_PREFIX2`) and restart
Prefix3         | \<prefix\>     | Set MQTT telemetry prefix (10 chars max) and restart
SensorRetain    |                | Show current sensor MQTT retain flag state
SensorRetain    | 0 / off        | (default) Disable use of sensor MQTT retain flag
SensorRetain    | 1 / on         | Enable MQTT retain flag on message tele/sonoff/SENSOR
SetOption2      |                | Show current Units state
SetOption2      | 0 / off        | (default) Do not show units to messages
SetOption2      | 1 / on         | Add units to messages
SetOption3      |                | Show current MQTT state
SetOption3      | 0 / off        | Disable MQTT
SetOption3      | 1 / on         | Enable MQTT
SetOption4      |                | Show current MQTT response state
SetOption4      | 0 / off        | Return response as RESULT topic
SetOption4      | 1 / on         | Return response as Command topic
SetOption10     |                | Show current LWT action when changing topic
SetOption10     | 0 / off        | (default) When topic changes drop retained old topic LWT
SetOption10     | 1 / on         | When topic changes send old topic retained LWT offline
StateText1      |                | Show current Off state text
StateText1      | \<text\>       | Set Off state text (10 chars max)
StateText2      |                | Show current On state text
StateText2      | \<text\>       | Set On state text (10 chars max)
StateText3      |                | Show current Toggle state text
StateText3      | \<text\>       | Set Toggle state text (10 chars max)
StateText4      |                | Show current button Hold state text
StateText4      | \<text\>       | Set button Hold state text (10 chars max)
SwitchRetain    |                | Show current button MQTT retain flag state
SwitchRetain    | 0 / off        | (default) Disable use of MQTT retain flag
SwitchRetain    | 1 / on         | Set ButtonTopic to Topic and enable MQTT retain flag on button press
SwitchTopic     |                | Show current MQTT switch topic
SwitchTopic     | 0 / off        | Disable use of MQTT switch topic
SwitchTopic     | 1              | Set MQTT switch topic to Topic
SwitchTopic     | \<topic\>      | Set MQTT switch topic (32 chars max)
TelePeriod      |                | Show current telemetry period in seconds
TelePeriod      | 0 / off        | Disable telemetry messages
TelePeriod      | 1              | Reset telemetry period to `user_config.h` (`TELE_PERIOD`)
TelePeriod      | \<secs\>       | Set telemetry period between 10 and 3600 seconds
Topic           |                | Show current MQTT topic
Topic           | 1              | Reset MQTT topic to `user_config.h` (`MQTT_TOPIC`) and restart
Topic           | \<topic\>      | Set MQTT topic (32 chars max) AND button topic and restart
Units           |                | Replaced by SetOption2


### SetOption Overview

Command     | Payload        | Description
------------|----------------|--------------------------------------------------------------
SetOption0  |                | Show current Save power changes state
SetOption0  | 1 / on         | (default) Save power changes and set relay after restart
SetOption0  | 0 / off        | Do not save power changes and do not set relay after restart
SetOption1  |                | Show current button multi press mode
SetOption1  | 0 / off        | (default) Allow all button actions
SetOption1  | 1 / on         | Allow only single, double and hold press button actions
SetOption2  |                | Show current Units state
SetOption2  | 0 / off        | (default) Do not show units to messages
SetOption2  | 1 / on         | Add units to messages
SetOption3  |                | Show current MQTT state
SetOption3  | 0 / off        | Disable MQTT
SetOption3  | 1 / on         | Enable MQTT
SetOption4  |                | Show current MQTT response state
SetOption4  | 0 / off        | Return response as RESULT topic
SetOption4  | 1 / on         | Return response as Command topic
SetOption8  |                | Show current Temperature as either Celsius or Fahrenheit
SetOption8  | 0 / celsius    | Set Temperature to Celsius
SetOption8  | 1 / fahrenheit | Set Temperature to Fahrenheit
SetOption10 |                | Show current LWT action when changing topic
SetOption10 | 0 / off        | (default) When topic changes drop retained old topic LWT
SetOption10 | 1 / on         | When topic changes send old topic retained LWT offline
SetOption11 |                | Show current button single and double press swap state
SetOption11 | 0 / off        | (default) Legacy button single and double press
SetOption11 | 1 / on         | Button single and double press functionality swapped
SetOption12 |                | Show current configuration flash usage option
SetOption12 | 0 / off        | (default) Use dynamic flash to save configuration lowering flash wear
SetOption12 | 1 / on         | Legacy save configuration in eeprom flash location only
SetOption13 |                | Show current button single press only state
SetOption13 | 0 / off        | (default) Enable button single, multi-press and hold
SetOption13 | 1 / on         | Enable button single press only for immediate response. Disable by holding 4 x SetOption32
SetOption14 |                | Show current relay switch mode
SetOption14 | 0 / off        | (default) Set self-locking mode for all relays
SetOption14 | 1 / on         | Set interlock mode for all relays 
SetOption32 |                | Show current key hold time in 0.1 seconds
SetOption32 | 1..100         | Set key hold time from 0.1 to 10 seconds
SetOption33 |                | Show Sonoff Pow Max_Power_Retry value
SetOption33 | 1..250         | Set Sonoff Pow Max_Power_Retry from 1 to 250 


### Logging

Command         | Payload   | Description
----------------|-----------|--------------------------------------------------------
LogHost         |           | Show current syslog host
LogHost         | 1         | Reset syslog host to `user_config.h` (`SYS_LOG_HOST`)
LogHost         | \<host\>  | Set syslog host (32 chars max)
LogPort         |           | Show current syslog port
LogPort         | 1         | Reset syslog port to `user_config.h` (`SYS_LOG_PORT`)
LogPort         | \<port\>  | Set syslog port between 2 and 32766
SerialLog       |           | Show current serial log level
SerialLog       | 0 / off   | Disable serial logging
SerialLog       | 1         | Show only error messages
SerialLog       | 2         | Show error and info messages
SerialLog       | 3         | Show error, info and debug messages
SerialLog       | 4         | Show all messages
SysLog          |           | Show current syslog level
SysLog          | 0 / off   | Disable syslog logging
SysLog          | 1         | Show only error messages
SysLog          | 2         | Show error and info messages
SysLog          | 3         | Show error, info and debug messages
SysLog          | 4         | Show all messages
WebLog          |           | Show current serial log level
WebLog          | 0 / off   | Disable Web logging
WebLog          | 1         | Show only error messages
WebLog          | 2         | Show error and info messages
WebLog          | 3         | Show error, info and debug messages
WebLog          | 4         | Show all messages
CfgDump         |           | Dump all configuration to log file


### Sonoff Pow

Command         | Payload       | Description
----------------|---------------|----------------------------------------------------------
CurrentHigh     |               | Show current current high threshold value
CurrentHigh     | 0 / off       | (default) Disable current high threshold
CurrentHigh     | \<milliamps\> | Set current high threshold value
CurrentLow      |               | Show current current low threshold value
CurrentLow      | 0 / off       | (default) Disable current low threshold
CurrentLow      | \<milliamps\> | Set current low threshold value
EnergyRes       |               | Show current Energy Resolution
EnergyRes       | 0..5          | Set Energy Resolution
EnergyReset     |               | Show Energy Total, Yesterday and Today
EnergyReset     | 1             | Reset Energy Today
EnergyReset     | 2             | Reset Energy Yesterday
EnergyReset     | 3             | Reset Energy Total to value of Energy Today
HlwIcal         |               | Show calibration value for Current
HlwIcal         | 1             | Set default calibration value for Current to 3500
HlwIcal         | \<value\>     | Set user calibration value for Current starting from 1100
HlwIset         | \<milliamps\> | Auto-calibrate current to a target value
HlwPcal         |               | Show calibration value for Power
HlwPcal         | 1             | Set default calibration value for Power to 12530
HlwPcal         | \<value\>     | Set user calibration value for Power starting from 4000
HlwPset         | \<watts\>     | Auto-calibrate power to a target value
HlwUcal         |               | Show calibration value for Voltage
HlwUcal         | 1             | Set default calibration value for Voltage to 1950
HlwUcal         | \<value\>     | Set user calibration value for Voltage starting from 1000
HlwUset         | \<volts\>     | Auto-calibrate voltage to a target value
MaxPower        |               | Show current maximum power allowed setting
MaxPower        | 0 / off       | Disable use maximum power monitoring
MaxPower        | \<watt\>      | Set maximum allowed power
MaxPowerHold    |               | Show current time in seconds to stay over MaxPower before power off
MaxPowerHold    | 1             | Set default time to 10 seconds to stay over MaxPower before power off
MaxPowerHold    | \<seconds\>   | Set time in seconds to stay over MaxPower before power off
MaxPowerWindow  |               | Show current time in seconds to stay power off before re-applying power up to 5 times
MaxPowerWindow  | 1             | Set default time to 30 seconds to stay power off before re-applying power up to 5 times
MaxPowerWindow  | \<seconds\>   | Set time in seconds to stay power off before re-applying power up to 5 times
PowerHigh       |               | Show current power high threshold value
PowerHigh       | 0 / off       | (default) Disable power high threshold
PowerHigh       | \<watt\>      | Set power high threshold value
PowerLow        |               | Show current power low threshold value
PowerLow        | 0 / off       | (default) Disable power low threshold
PowerLow        | \<watt\>      | Set power low threshold value
SetOption33     |               | Show Sonoff Pow Max_Power_Retry value
SetOption33     | 1..250        | Set Sonoff Pow Max_Power_Retry from 1 to 250 
Status          | 8             | Show Power usage
Status          | 9             | Show Power thresholds
VoltageHigh     |               | Show current voltage high threshold value
VoltageHigh     | 0 / off       | (default) Disable voltage high threshold
VoltageHigh     | \<voltage\>   | Set voltage high threshold value
VoltageLow      |               | Show current voltage low threshold value
VoltageLow      | 0 / off       | (default) Disable voltage low threshold
VoltageLow      | \<voltage\>   | Set voltage low threshold value
VoltRes         |               | Show current Voltage Resolution
VoltRes         | 0..1          | Set Voltage Resolution
WattRes         |               | Show current Power Resolution
WattRes         | 0..1          | Set Power Resolution


### AiLight, Sonoff Led, B1 and BN-SZ01

Command         | Payload   | Description
----------------|-----------|--------------------------------------------------------
Color           |           | Show current color setting as CCWW, RRGGBBWW or RRGGBBCCWW
Color           | \<CCWW\>  | (Sonoff Led) Set color to CCWW hexadecimal value
Color           | \<RRGGBBWW\> | (AiLight) Set color to RRGGBBWW hexadecimal value
Color           | \<RRGGBBCCWW\> | (Sonoff B1) Set color to RRGGBBCCWW hexadecimal value
CT              |           | (Sonoff B1 and Led) Show current Color Temperature (153 = Cold, 500 = Warm)
CT              | 153..500  | (Sonoff B1 and Led) Set color temperature from cold to warm
Dimmer          |           | Show current dimmer setting from 0 to 100%
Dimmer          | 0..100    | Set dimmer value from 0 to 100%
Fade            |           | Show current fade state
Fade            | 0 / off   | (default) Do not use fade
Fade            | 1 / on    | Use fade
LedTable        |           | Show current Led table intensity correction state
LedTable        | 0 / off   | (default) Do not use Led table for intensity correction
LedTable        | 1 / on    | Use Led table for intensity correction
Speed           |           | Show current fade speed selection
Speed           | 1..8      | Select desired fade speed from 1 = fast to 8 = slow
Wakeup          |           | Start wake up sequence from Off to Dimmer value
WakeupDuration  |           | Show current wake up light duration in seconds
WakeupDuration  | 1..3600   | Set wake up light duration in seconds


### Sonoff RF Bridge 433

Command      | Payload     | Description
-------------|-------------|----------------------------------------------------------
RfKey\<x\>   |             | Send learned or default RF data for RfKey1 to RfKey16
RfKey\<x\>   | 1           | Send default RF data for RfKey1 to RfKey16
RfKey\<x\>   | 2           | Learn RF data for RfKey1 to RfKey16
RfKey\<x\>   | 3           | Unlearn RF data for RfKey1 to RfKey16


### Domoticz

Command                | Payload | Description
-----------------------|---------|---------------------------------------------------------------------------
DomoticzIdx\<x\>       |         | Show current Domoticz relay1 to relay4 index
DomoticzIdx\<x\>       | 0 / off | (default) Disable use of Domoticz
DomoticzIdx\<x\>       | \<idx\> | Set Domoticz relay1 to relay4 index
DomoticzKeyIdx\<x\>    |         | Show current Domoticz key1 to key4 index
DomoticzKeyIdx\<x\>    | 0       | (default) Disable use of key1 to key4 index
DomoticzKeyIdx\<x\>    | \<idx\> | Set Domoticz key1 to key4 index. To use it you'll need to enable ButtonTopic too
DomoticzSensorIdx\<x\> |         | Show current Domoticz sensor1 to sensor5 index
DomoticzSensorIdx\<x\> | 0       | (default) Disable use of sensor1 to sensor5 index
DomoticzSensorIdx\<x\> | \<idx\> | Set Domoticz sensor1 to sensor5 index.
DomoticzSwitchIdx\<x\> |         | Show current Domoticz switch1 to switch4 index
DomoticzSwitchIdx\<x\> | 0       | (default) Disable use of switch1 to switch4 index
DomoticzSwitchIdx\<x\> | \<idx\> | Set Domoticz switch1 to switch4 index. To use it you'll need to enable SwitchTopic too
DomoticzUpdateTimer    |         | Show current Domoticz update timer value in seconds
DomoticzUpdateTimer    | 0 / off | (default) Disable sending interrim Domoticz status
DomoticzUpdateTimer    | 1..3600 | Send status to Domoticz between every 1 and 3600 seconds


### WS2812 Led string

Command            | Payload    | Description
-------------------|------------|------------------------------------------------------
Color              |            | Show current strip/ring color setting as RRGGBB
Color              | \<RRGGBB\> | Set strip/ring color to RRGGBB hexadecimal value
Dimmer             |            | Show current dimmer setting from 0 to 100%
Dimmer             | 0..100     | Set dimmer value from 0 to 100%
Fade               |            | Show current color fade state
Fade               | 0 / off    | (default) Do not use fade while changing colors
Fade               | 1 / on     | Use fade while changing colors
Led1..Led\<count\> |            | Show specific led current color as RRGGBB
Led1..Led\<count\> | \<RRGGBB\> | Set specific led to desired color RRGGBB
LedTable           |            | Show current Led table color correction state
LedTable           | 0 / off    | (default) Do not use Led table for color correction
LedTable           | 1 / on     | Use Led table for color correction
Pixels             |            | Show current pixel count
Pixels             | \<count\>  | Set amount of pixels in strip or ring up to 256
Scheme             |            | Show current selected scheme
Scheme             | 0          | (default) Use single color for all leds in strip/ring
Scheme             | 1          | Start wakeup light
Scheme             | 2          | Show clock
Scheme             | 3          | Show incandescent pattern
Scheme             | 4          | Show rgb pattern
Scheme             | 5          | Show Christmas pattern
Scheme             | 6          | Show Hanukkah pattern
Scheme             | 7          | Show Kwanzaa pattern
Scheme             | 8          | Show rainbow pattern
Scheme             | 9          | Show fire pattern
Speed              |            | Show current fade speed selection
Speed              | 1..5       | Select desired fade speed from 1 = fast to 5 = slow
Wakeup             |            | Show current wake up light duration in seconds
Wakeup             | 1..3600    | Set wake up light duration in seconds
Width              |            | Show current led group width
Width              | 0..4       | Set led group width used by Schemes 3 - 9


### IRremote

| Command | Payload                   | Description
| --------|---------------------------|------------------------------------------------------
| IRsend  | `{"Protocol": "<proto>",` | Send IR remote control as JSON encapsulated command.
|         |  `"Bits": 1..32`          | \<proto\> is NEC, SONY, RC5, RC6, DISH, JVC or SAMSUNG
|         |  `"Data": 1..(2^32)-1}`   | bits are the required number of data bits.
|         |                           | data is the data frame as 32 bit unsigned integer.
|         |                           | See http://www.lirc.org/ for more info.
|         |                           |
| IRhvac  | `{"Vendor": "<Toshiba/Mitsubishi>",`       | Send IR remote control data to Toshiba
|         |  `"Power": <0/1>,`                         | or Mitsubishi HVAC
|         |  `"Mode": "<Hot/Cold/Dry/Auto>",`          |
|         |  `"FanSpeed": "<1/2/3/4/5/Auto/Silence>",` |
|         |  `"Temp": <17..30>}`                       |
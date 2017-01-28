The button on sonoff provides the following features

### Version 2.0.3 and up:

- a short press toggles the relay either directly or by sending a MQTT message like ```cmnd/sonoff/1/light on```. This will blink the LED twice and sends a MQTT status message like ```stat/sonoff/LIGHT on```. If ```cmnd/sonoff/ButtonRetain on``` has been used the MQTT message will also contain the MQTT retain flag.
- two short presses toggles the relay. This will blink the LED twice and sends a MQTT status message like ```stat/sonoff/POWER on```. For Sonoff Dual this will switch relay 2.
- three short presses start Wifi smartconfig allowing for SSID and Password configuration using an Android mobile phone with the [ESP8266 SmartConfig](https://play.google.com/store/apps/details?id=com.cmmakerclub.iot.esptouch) app. The MQTT server still needs to be configured in the ```user_config.h``` file. The LED will blink during the config period. A single button press during this period will abort and restart sonoff.
- four short presses start Wifi manager providing an Access Point with IP address 192.168.4.1 and a web server allowing the configuration of both Wifi and MQTT parameters. The LED will blink during the config period. A single button press during this period will abort and restart sonoff.
- five short presses start Wifi Protected Setup (WPS) allowing for SSID and Password configuration using the routers WPS button or webpage. The LED will blink during the config period. A single button press during this period will abort and restart sonoff.
- six short presses will restart sonoff
- seven short presses start OTA download of firmware. The green LED is lit during the update
- pressing the button for over four seconds resets settings to defaults as defined in ```user_config.h``` and restarts the device

### Version 2.0.2 down to 2.0.0:

- a short press toggles the relay either directly or by sending a MQTT message like ```cmnd/sonoff/1/light 2```. This will blink the LED twice and sends a MQTT status message like ```stat/sonoff/LIGHT on```.
- two short presses toggles the relay. This will blink the LED twice and sends a MQTT status message like ```stat/sonoff/POWER on```. For Sonoff Dual this will switch relay 2.
- three short presses start Wifi smartconfig allowing for SSID and Password configuration using an Android mobile phone with the [ESP8266 SmartConfig](https://play.google.com/store/apps/details?id=com.cmmakerclub.iot.esptouch) app. The MQTT server still needs to be configured in the ```user_config.h``` file. The LED will blink during the config period. A single button press during this period will abort and restart sonoff.
- four short presses start Wifi manager providing an Access Point with IP address 192.168.4.1 and a web server allowing the configuration of both Wifi and MQTT parameters. The LED will blink during the config period. A single button press during this period will abort and restart sonoff.
- five short presses start Wifi Protected Setup (WPS) allowing for SSID and Password configuration using the routers WPS button or webpage. The LED will blink during the config period. A single button press during this period will abort and restart sonoff.
- six short presses will restart sonoff
- seven short presses start OTA download of firmware. The green LED is lit during the update
- pressing the button for over four seconds resets settings to defaults as defined in ```user_config.h``` and restarts the device

### Version 1.0.35 down to 1.0.32:

- a short press toggles the relay either directly or by sending a MQTT message like ```cmnd/sonoff/light 2```. This will blink the LED twice and sends a MQTT status message like ```stat/sonoff/LIGHT on```
- two short presses toggles the relay. This will blink the LED twice and sends a MQTT status message like ```stat/sonoff/POWER on```
- three short presses start Wifi smartconfig allowing for SSID and Password configuration using an Android mobile phone with the [ESP8266 SmartConfig](https://play.google.com/store/apps/details?id=com.cmmakerclub.iot.esptouch) app. The MQTT server still needs to be configured in the ```user_config.h``` file. The LED will blink during the config period. A single button press during this period will abort and restart sonoff.
- four short presses start Wifi manager providing an Access Point with IP address 192.168.4.1 and a web server allowing the configuration of both Wifi and MQTT parameters. The LED will blink during the config period. A single button press during this period will abort and restart sonoff.
- five short presses start Wifi Protected Setup (WPS) allowing for SSID and Password configuration using the routers WPS button or webpage. The LED will blink during the config period. A single button press during this period will abort and restart sonoff.
- six short presses will restart sonoff
- seven short presses start OTA download of firmware. The green LED is lit during the update
- pressing the button for over four seconds resets settings to defaults as defined in ```user_config.h``` and restarts the device

### Version 1.0.31 down to 1.0.22:

- a short press toggles the relay either directly or by sending a MQTT message like ```cmnd/sonoff/light 2```. This will blink the LED twice and sends a MQTT status message like ```stat/sonoff/LIGHT on```
- two short presses toggles the relay. This will blink the LED twice and sends a MQTT status message like ```stat/sonoff/POWER on```
- three short presses start Wifi smartconfig allowing for SSID and Password configuration using an Android mobile phone with the [ESP8266 SmartConfig](https://play.google.com/store/apps/details?id=com.cmmakerclub.iot.esptouch) app. The MQTT server still needs to be configured in the ```user_config.h``` file. The LED will blink during the config period. A single button press during this period will abort and restart sonoff.
- four short presses start Wifi manager providing an Access Point with IP address 192.168.4.1 and a web server allowing the configuration of both Wifi and MQTT parameters. The LED will blink during the config period. A single button press during this period will abort and restart sonoff.
- five short presses will restart sonoff
- six short presses start OTA download of firmware. The green LED is lit during the update
- pressing the button for over four seconds resets settings to defaults as defined in ```user_config.h``` and restarts the device

### Version 1.0.21 and down:

- a short press toggles the relay either by sending a MQTT message like ```cmnd/sonoff/light 2``` or directly. This will blink the LED twice and sends a MQTT status message like ```stat/sonoff/LIGHT on```
- two short presses toggles the relay. This will blink the LED twice and sends a MQTT status message like ```stat/sonoff/POWER on```
- three short presses start Wifi smartconfig which allows for SSID and Password configuration using an Android mobile phone with the [ESP8266 SmartConfig](https://play.google.com/store/apps/details?id=com.cmmakerclub.iot.esptouch) app. The green LED will blink during the smartconfig period for 1 minute. The MQTT server still needs to be configured in the ```user_config.h``` file
- four short presses start OTA download of firmware. The green LED is lit during the update
- five short presses will restart sonoff
- pressing the button for over four seconds resets settings to defaults as defined in ```user_config.h``` and restarts the device

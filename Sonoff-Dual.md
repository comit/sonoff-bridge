## Solving intermittent relay switch errors
Where most Sonoff's use GPIO to control one or more relays the Sonoff Dual and 4 Channel Inching Relay Assy do use the standard SERIAL interface to control the relays.

Commands are send from the ESP8266 via a 19200 baud serial connection to a dedicated chip that controls the relays.

It is therefore important to disable any serial communication to and from the device once you have debugged any anomalies.

To assist easy installation serial logging is enabled by default in user_config.h for all Sonoffs. Once in production it's wise to turn it off for all Sonoffs. For the Dual it is almost mandatory to turn it off.

Execute command ```seriallog 0``` once to turn all communication on the serial port off.

If within 10 minutes no input is received serial communication is turned off too. 
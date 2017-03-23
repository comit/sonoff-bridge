# Common issues (in no particular order):

## Relay clicks/LED flashes at 1 sec interval
This indicates that the ESP-8266 did not get flashed properly. In this case it will toggle all it's pins at 1 sec intervals

## Running out of memory
This typically shows up in the device working when it first starts up (hitting the button toggles the relay), but some time later it either reboots or some function won't work.

For example, you can't load the module configuration page.

The only fix for this is to recompile the firmware and disable features you don't need.

Known large features are webserver and TLS, but other things to consider disabling if you don't need them are emulation support, domoticz support, ws8212 support

## Config problems (can cause boot loops, items set in user_config to not appear, etc)
By default, this firmware tries to preserve the existing config (to support automated updates via OTA upgrades), but various things can happen that cause the existing config to be a problem.

There are multiple ways to force the config to what's set in user_config to recover a system.

1. hold button1 down for 4 seconds
1. issue a reset command (reset 1 via the web console, /cmnd/sonoff/reset 1 via mqtt)
1. change the value of CFG_HOLDER in user_config and re-flash the device

# troubleshooting tools
## logs
The logs are available via syslog, the web console, or the serial port.

The default logging level is set in user_config* and is set separately for each log destination
The higher the log level is set, the more information is logged.

### Syslog
If you have a Linux system around (say a Pi), it is probably running syslog already and you just need to configure it to listen on the network.

On systems running rsyslog (most linux distros), edit the file /etc/rsyslog.conf. Adding (or uncommenting) the following lines will probably start making the logs show up in some file under /var/log

$ModLoad imudp
$UDPServerRun 514
### Web Logs
These show up on the web console (http://device/cs)
### Serial Logs
WARNING, on sonoff POW you cannot be connected to the serial port while the device is connected to mains power. You can still collect the logs, but only when powering it via your serial connection
The sonoff dual uses the serial port to control the relays, so serial logging interferes with the relay control

Through either a terminal program or the android serial monitor, set the baud rate to 115200 and disable hardware flow control and you will see the logs

## crashdumps
If the ESP-8266 crashes, it frequently dumps information about the crash out the serial port, so the process listed above to see serial logs can provide extremely useful information
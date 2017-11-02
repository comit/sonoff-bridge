# Common issues (in no particular order):

## Uploading Sketch via FTDI/UART Not Working

Testing with two different (fairly new) FTDI boards and two new (Purchased 28/06/2017) Sonoff 4CH v2.0 and the Sonoff Dual v2.0 boards I found that I was getting errors uploading sketches i.e. "warning: espcomm_sync failed" basically a lack of communication between the two devices.

I found that the problem in both Sonoff's was that instead of the FTDI Sonoff cross-over TX->RX and RX->TX I had to do 
TX->TX RX->RX this then allowed me to upload the sketch.

## Wifi stops working
There have been many reports of wifi no longer working when it was working for a while.

Every time this has been reported, it's ended up being a hardware or interference problem.

On the hardware side, we've seen reports of bad solder joints on the board that when touched up seem to solve the problem (capacitors being loose can cause this)

We've also seen reports then when a specific LED light bulb was hooked up near one, the signal quality dropped to unusable.

All you can really do is check the solder joints, move them around (try hooking up different loads), and if all else fails, replace the units.

## Relay clicks/LED flashes at 1 sec interval
This indicates that the ESP-8266 did not get flashed properly. In this case it will toggle all it's pins at 1 sec intervals

## Running out of memory
This typically shows up in the device working when it first starts up (hitting the button toggles the relay), but some time later it either reboots or some function won't work.

For example, you can't load the module configuration page.

The only fix for this is to recompile the firmware and disable features you don't need.

Known large features are webserver and TLS, but other things to consider disabling if you don't need them are emulation support, domoticz support, ws8212 support

## Config problems (can cause boot loops, items set in user_config to not appear, etc)

By default, this firmware tries to preserve the existing config (to support automated updates via OTA upgrades), but various things can happen that cause the existing config to be a problem.

When you change the settings in the code, that doesn't directly change the
settings on the running machine when you load it. What happens is that when it  
boots up, the firmware looks to see if it has a valid config (is it an upgrade  
to an older tasmota version), and if the CFG_HOLDER value is in the right 
place,it assumes that the existing config is valid. If it doesn't find the right
value, it assumes that this is not an upgrade and takes the compiled-in config  
data and writes it out to the config area.

There are multiple ways to force the config to what's set in user_config to recover a system.

1. hold button1 down for 4 seconds
1. issue a reset command (``reset 1`` via the web console, ``/cmnd/sonoff/reset 1`` via mqtt)
1. change the value of CFG_HOLDER in user_config and re-flash the device

### esptool usage
Clearing the configuration flash area can also solve unbootable systems. Using the latest esptool supported by espressif makes this process "rather" easy.

1. Go to https://github.com/espressif/esptool and read the README.md regarding installing esptool which comes down to
  - Install python 2.7.x for your operating system from https://www.python.org/
  - Open a command prompt and install pyserial with command<br/> ``pip install pyserial``
  - Install esptool with command<br/> ``pip install esptool``
2. Clear the Tasmota configuration flash area
  - Connect your device to a known serial port (say COM5)
  - Hold the push button and apply 3V3 power to the device from the USB/serial connecting device (ie FTDI)
  - Open a command prompt and execute command<br/> ``esptool.py --port COM5 erase_region 0x0F4000 0x008000``
3. Optional Clear the complete flash with command<br/> ``esptool.py --port COM5 erase_flash``
4. Optional Load Tasmota into a device with command<br/> ``esptool.py --port COM5 write_flash -fs 1MB -fm dout 0x0 sonoff.bin``

### PlatformIO + esptool
With the Command Prompt execute:
1. cd %USERPROFILE%\.platformio\python27\Scripts\
2. pip install pyserial
3. pip install esptool

After this you can use the same commands as above:
1. esptool --port COM5 erase_region 0x0F4000 0x008000
2. esptool --port COM5 erase_flash
3. esptool --port COM5 write_flash --flash_mode dout --flash_size 1MB 0x0 firmware.bin

## does not respond to button intermittently.
The library that is being used to make the TCP connection to the MQTT server has a 5 second timeout, during which the firmware is stuck and can do nothing else (including switching the relay locally)

When the connection fails, the firmware can then operate locally for a little bit until it attempts a new connection.

Note that if it has no network connection at all, this problem doesn't happen, because it detects it doesn't have a network to try and connect over, and local operation can work without delays.

## Disconnects and Reconnects

If the serial debugger shows repeated messages like this -
```
02:32:54 MQTT: tele/MYSONOFF/LWT = Online (retained)
02:32:54 MQTT: cmnd/MYSONOFF/POWER = 
02:32:55 MQTT: Attempting connection...
02:32:56 mDNS: Query done with 0 mqtt services found
02:32:56 MQTT: Connected
```
or your mosquitto log shows messages like this -
```
1496455347: New client connected from IP_addr_1 as SONOFF (c1, k15, u'SONOFF_USER').
1496455349: New connection from IP_addr_1 on port 1883.
1496455349: Client SONOFF already connected, closing old connection.
1496455349: Client SONOFF disconnected.
1496455349: New client connected from IP_addr_2 as SONOFF (c1, k15, u'SONOFF_USER').
1496455350: New connection from IP_addr_2 on port 1883.
1496455350: Client SONOFF already connected, closing old connection.
1496455350: Client SONOFF disconnected.
```
Then you have more than one device connected with the same client_ID. Its important that each device has a unique client_ID, not just PROJECT

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

## unknown issues
    %USERPROFILE%.platformio\python27\Scripts>esptool --port COM12 write_flash --flash_mode dout --flash_size 1MB 0x0 
    firmware.bin
    esptool.py v2.1
    Connecting....
    Detecting chip type... ESP8266
    Chip is ESP8266
    Uploading stub...
    Running stub...
    Stub running...
    Configuring flash size...
    Compressed 475008 bytes to 327693...
    Writing at 0x00008000... (14 %)
    A fatal error occurred: Timed out waiting for packet header
Maybe because off: https://vilimpoc.org/blog/2016/05/03/esptool-ck-esp8266-and-ftdi-bug-hunting/
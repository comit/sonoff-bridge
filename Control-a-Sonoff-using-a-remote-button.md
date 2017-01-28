### REMOTE BUTTON

I have a cabin that is remote from my house, and I'm using a Sonoff to turn on/off the heaters remotely, so the place can be warm when I get there. The Sonoff(heater control) is located near the fuse box, and it controls a relay which switches the 120V/30A to the heaters.

It's awkward to get at the Sonoff(heater control) to physically press the toggle button, so when I'm using it, I use MQTT commands to control the heaters. But I have some family members that are not comfortable with MQTT usage and certainly don't want to open the fuse box.

So I built a small ESP-01 and a DHT22 and a push button, and used this project code to drive it (>rev 1.0.30). I use the `cmnd/my_DHT/buttontopic` to set the mqtt command to control the heaters. Yes, you can program a sonoff so that the button sends out any arbitrary command. In this case, I'm using the button to control the other sonoff. This ESP+DHT device is in the common area of the cabin, so my family members can simply push the button to toggle the heater.

In other words, I'm using _one_ trivial (DHT+button) wifi device to control the _other_ (sonoff+heater relay). This technique uses Theo's flexible design, since _buttontopic_ can be made different from _topic_.

### Schematic ![schematic of trivial DHT + ESP01](http://alt.pbeirne.com/images/esp_dht2.png)
NOTE: with this schematic, the DHT sensor is on GPIO2. You'll have to change the` #define DHT_PIN ` in this project's source code from 14 to 2. Also, make sure you use the `black` ESP01 with 1MB of flash; most 2016 purchases should be ok. Of course you can use an ESP12 if you wish.

The optional block shown attaches an LED to the DHT data line, so you can watch when the data is being fetched from the DHT device.

Power regulator, 5V to 3.3V: search eBay for `5v/3.3v power 1117`<br>
ESP: search eBay for `esp-01` and pick a black one<br>
DHT22: search eBay for `dht22` ; any AM2302, DHT11 or DHT12 will do in its place; or you can omit the DHT and just use the button<br>
Cost (2016-09-05) USB-5V-adapter: CAD$1 + DC5V->3.3V: CAD$1 + ESP01: CAD$2.62 + DHT12: CAD$1.50 = CAD$6.12 = EUR 4.25

This little device has two _inputs_ (DHT & button), and no _output_. You can use either the Sonoff or the ElectroDragon version of this project, but because this has no output, the following commands don't make any sense: `power, light, ledstate`. You may also want to change the `#define APP_NAME` and `#define PROJECT` to reflect that this isn't a real Sonoff.

Pat B
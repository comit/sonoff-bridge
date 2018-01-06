
**Sonoff T1 is currently unsupported by Tasmota**

This is the EU-Version

* Itead Product Page: http://sonoff.itead.cc/en/products/residential/sonoff-t1
* Itead Shop: https://www.itead.cc/sonoff-t1.html
* Itead Wiki: (na)

![Sonoff T1 Image](https://cdn.itead.cc/media/catalog/product/cache/1/thumbnail/160x160/9df78eab33525d08d6e5fb8d27136e95/s/o/sonoff_t1_000.jpg)

Sonoff-T1 seems to be an evolution of Sonoff-Touch, and exists in one, two or three button variations and contains a 433MHz radio.

Sonoff Touch is also based on the ESP8285, But are using a Silabs EFM8BB1 microcontroller to extend the number of IO:s needed to control 3 buttons, with separate relays and leds together with the radio. SYN470R is used as 433Mhz Radio, and chip for touch is unlabeled


[EFM8BB1 Data Sheet](https://www.silabs.com/documents/public/data-sheets/efm8bb1-datasheet.pdf)

[ESP8385 Data Sheet](http://www.espressif.com/sites/default/files/documentation/0a-esp8285_datasheet_en.pdf)

[SYN470R Data Sheet](https://www.birdandgua.net/bird/wp-content/uploads/2016/09/SYN470R-Synoxo.pdf)

## Serial Connection

There is 3 connectors available on the top board J1 to J3.
J1 is tied to EFM8BB1:
1. VDD
2. DATA
3. CLOCK
4. GND

J2 goes to the relay board (Pins counted cw):
1. Relay 3
2. Relay 1
7. Relay 2
8. Some kind of relay feedback.

J3 is a serial pinout, with the same pins as sonoff basic:
1. VDD
2. RX
3. TX
4. GND
5. GPIO02

With standard firmware, i get a short print out with an non-standard board rate: 75 117bps (Meassured with logic analyzer) and then silence.

```
 ets Jan  8 2013,rst cause:1, boot mode:(3,7)

load 0x40100000, len 2408, room 16
tail 8
chksum 0xe5
load 0x3ffe8000, len 776, room 0
tail 8
chksum 0x84
load 0x3ffe8310, len 632, room 0
tail 8
chksum 0xd8
csum 0xd8

2nd boot version : 1.6
  SPI Speed      : 40MHz
  SPI Mode       : DOUT
  SPI Flash Size & Map: 8Mbit(512KB+512KB)
jump to run user1 @ 1000

rf cal sector: 251
rf[112]▒ector: 251@ 10008Mbit(512KB+512KB) mode:(3,7)
```

## Flashing
Pressing first touch button, will make GPIO0 go low. I Had no luck so fare get it to flash

## Circuit
I tried to reverse engineer the circuit and i noticed:

ESP8285
```

GPIO0 EFM8BB1 P1,3 (Goes low when first touch button is pressed)
GPIO04 is connected to the small pairing button on the front.
GPIO09 EFM8BB1 P1,4
GPIO10 EFM8BB1 P1,5
GPIO13 is connected to status LED D3.
```

On the EFM8BB1 (QFN20 package)
```
P0,0 Relay 1
P0,1 Relay 2
P0,2 Relay 3
P0,3 Button 1
P0,4 Button 2
P0,5 Button 3
P0,6 SYN470R Data Out
P0,7 Relay protection?
P1,0 Led button 1
P1,1 Led button 2
P1,2 Led button 3
P1,3 ESP8285 GPIO0
P1,4 ESP8285 GPIO09
P1,5 ESP8285 GPIO10
P1,6 ESP8285 EXT_RSTB (RESET)
```

## Known so far
* It seems that the state of the 3 relays are saved inside the EFM8BB1, and not inside the ESP8285.
* When pushing a button, the touch chip lift the power high to the EFM8BB1, and the EFM8BB1 chip ties the signal line for each button low, for the full duration of the keypress. There is no serial data, that i have seen at all.

## Unknown so far

* I have no way to put the chip into download mode.
* Why is my chip using such a strange serial bitrate, is it broken?
* How does the ESP8285 change and read the the state inside EFM8BB1. We only know that the ESP8285 can read if button is pressed or not, but now how the ESP8285 changes or reads the state.
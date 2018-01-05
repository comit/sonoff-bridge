
**Sonoff T1 is currently unsupported by Tasmota**

* Itead Product Page: http://sonoff.itead.cc/en/products/residential/sonoff-t1
* Itead Shop: https://www.itead.cc/sonoff-t1.html
* Itead Wiki: (na)

![Sonoff T1 Image](https://cdn.itead.cc/media/catalog/product/cache/1/thumbnail/160x160/9df78eab33525d08d6e5fb8d27136e95/s/o/sonoff_t1_000.jpg)

Sonoff-T1 seems to be an evolution of Sonoff-Touch, and exists in one, two or three button variations and contains a 433MHz radio.

Sonoff Touch is also based on the ESP8285, But are using a Silabs EFM8BB1 microcontroller to extend the number of IO:s needed to control 3 buttons, with separate relays and leds together with the radio.


[EFM8BB1 Data Sheet](https://www.silabs.com/documents/public/data-sheets/efm8bb1-datasheet.pdf)

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

## Flashing
It is unknown how to flash, as GPIO0 is directly tied to pin P1,3 to EFM8BB1 there is no known way to let ESP8285 enter download mode.

## Circuit
I tried to reverse engineer the circuit and i noticed:

ESP8285
```
GPIO01 EFM8BB1 P1,3?
GPIO04 is connected to the small pairing button on the front.
```

On the EFM8BB1
```
P0,0 Relay 1
P0,1 Relay 2
P0,2 Relay 3
P0,3 Button 1
P0,4 Button 2
P0,5 Button 3
P0,6
P0,7 Relay protection?
P1,0 Led button 1
P1,1 Led button 2
P1,2 Led button 3
P1,3 ESP8285 GPIO 0?
P1,4
P1,5
P1,6
```
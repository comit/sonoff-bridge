## MagicHome LED controllers (aka Flux-Led)

<img alt="MagicHome LED controller pads" src="https://user-images.githubusercontent.com/29731130/31029721-23312a68-a553-11e7-9bd9-0a45f38d3375.jpg" width="30%" align="right" /> 
<img alt="MagicHome LED controller pads" src="https://user-images.githubusercontent.com/29731130/31029726-269589f6-a553-11e7-95ed-aecb334d3aa3.jpg" width="30%" align="right" />

* https://www.aliexpress.com/item/Magic-Home-Mini-RGB-RGBW-Wifi-Controller-For-Led-Strip-Panel-light-Timing-Function-16million-colors/32686853650.html

Board is essentially a ESP-12S with necessary voltage converters, little bit of flash, 3 or 4 MOSFETs to drive LED strip (depending on the model), connector for LED strip and optional IR receiver. 

Module is powered by 12V that is used to power LED strip as well. Some models are declared as 144W, some other as 192W. Seems all use the same hardware though.

Module comes in (at least) 3 variants: 
- RGB, 
- RGBW and 
- RGBW with IR receiver.

## Serial Connection

Board has RX, TX, GND and GPIO00 pads exposed on the bottom side of the PCB. You need to solder temporary wires those pads.

<img alt="MagicHome LED controller pads" src="https://user-images.githubusercontent.com/29731130/31029735-2f99db9c-a553-11e7-896c-2f71c3d04551.jpg" width="50%" align="left" />

You need to power the board while keeping it connected to the programmer.

With all Sonoff boards that works with AC, this is a big no-no that will fry your programmer, your Sonoff and might even get you killed.
In this case, you'd be dealing with 12V, so the only thing that matters is to connect the GND of your programmer to GND of the board before you supply the 12V. Not doing so might fry your board and/or programmer, but would definitely not hurt you. 

Steps used: 
1. Connect your programmer to a breadboard and notice the locations of GND, TX and RX columns.
1. Open the MagicHome controller box and expose bottom side of PCB
1. Solder 4 jumper wires to 4 exposed pads.
1. **FIRST** connect GND to your programmer (and make sure they are connected well!)
1. Connect RX from the MagicHome to TX on the programmer. TX from the board goes to RX on the programmer.
1. Connect GPIO00 to GND (best to use same column on the breadboard)
1. Connect the 12V power supply to MagicHome. As GPIO00 is connected to GND, board will go into flash mode. I usually disconnect GPIO00 after few seconds, but not sure if this is mandatory or optional.
1. Upload Sonoff-Tasmota like it would be any other board.
1. Once upload is complete, disconnect power from the MagicHome controller
1. Disconnect RX and TX and then only then GND. GND gets disconnected **LAST.**

You can then connect the power back to the board and Sonoff-Tasmota should be running on it. Once you verify that board is up and you can access it over the Web, you can unsolder temporary wires and update subsequent firmware versions using OTA.

## Configuration 

Some GPIO are preconfigured with the board: 
- GPIO05 - Green color on the led strip, first pin from the GND
- GPIO14 - Red color on the LED strip, second pin from the GND
- GPIO12 - Blue color on the LED strip, third pin from the GND

Due to variants, you can configure:
- GPIO04 - on non-IR boards, it's open pin you can use for Onewire, button or something else. On IR-enabled boards, IR receiver is connected to this pin. Today, Sonoff doesn't use this receiver, but support for it is being worked upon.
- GPIO13  -  This pin is not used on RGB board (so you'll leave it as "None"), but on RGBW, it's driving another channel for LED strip. Depending on the strip type, you can set it to PWM4 (for RGB+Cold White) or PWM5 (for RGB+Warm White).

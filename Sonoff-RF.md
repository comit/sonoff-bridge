## Serial connection

The Sonoff RF needs some tweaking as the connection needed during programming between the button and GPIO0 might not be present.

[Phalox](http://phalox.be/wp/electronics/itead-sonoff-slampher-custom-firmware-fix/) explains the case with a picture where you have to install a jumper wire for R21. I received the same result using a small screwdriver and shorting both solder pads of R21 while holding down the button during programming.

<img alt="S20 Smart Socket" src="https://github.com/arendst/arendst.github.io/blob/master/media/sonoffrffix.jpg" width="230" align="right" />

Pairing the iTead RF remote controller is the same as with the original iTead software:
- two short button presses will blink a red led shortly and start RF signal recognition. Three longer blinks signal RF reception.
- three short button presses will keep the red led on for some seconds and erase the known RF code. 

I was unable to pair the Sonoff RF 434MHz receiver with my KaKu switches but the iTead provided remote control works just fine.


## Pairing a RF remote control
During programming a connection for R21 is needed in order to use a button press to ground GPIO0.

To pair a RF remote control with the Sonoff RF it is important that there is NO connection made for R21.

User [gadjet](https://github.com/gadjet) installed a jumper in place of R21 allowing easy programming (jumper in), pairing (jumper out) and normal use (jumper out).

<img alt="RF jumper" src="https://github.com/arendst/arendst.github.io/blob/master/media/sonoffrfjmpr.jpg" align="left" /> 

### Procedure
Pairing the iTead RF remote controller is the same as with the original iTead software:
- two short button presses will blink a red led shortly and start RF signal recognition. Three longer blinks signal RF reception.
- three short button presses will keep the red led on for some seconds and erase the known RF code. 

I was unable to pair the Sonoff RF 434MHz receiver with my KaKu switches but the iTead provided remote control works just fine.

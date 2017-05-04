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
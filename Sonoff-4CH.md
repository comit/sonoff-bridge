## Solving OTA and upload problems
Where most Sonoff's are ESP8266 based the Sonoff Touch and Sonoff 4CH are based on the ESP8285.

It is important to initially flash these devices with the correct FlashChipMode of DOUT as otherwise future OTA or Upload updates will fail.

In the Arduino IDE select **Board** Generic ESP8285 Module as this contains the default DOUT option. You could still use the Generic ESP8266 module as long as you set parameter ``Flash Mode`` to **DOUT**.

After first restart make sure to select either module Sonoff Touch or Sonoff 4CH at least before any OTA or Upload action.

Starting with version 3.9.12 the OTA or Upload code will now check if one of the above modules is selected and patch the uploaded ESP8266 code with FlashChipMode DIO for the ESP8285 FlashChipMode DOUT before it is copied to it's final destination.

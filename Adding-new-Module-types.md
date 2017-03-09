Module types are defined in sonoff_template.h

This includes a "User Test" module type that you can experiment with

You can add an additional type by editing both the mytmplt and module_t arrays to add your own module type (make sure you keep the order of entries in the two arrays the same)

The module definition is a list of what each of the 17 GPIO pins on the ESP-8266 are defined to be. Each item in the list can be either 0 (unused), or a pin definition from the list below.

As of 3/9/17 (4.0.2) these functions are:

* GPIO_KEY1 = GPIO_SENSOR_END,  // Button usually connected to GPIO0
* GPIO_KEY2,
* GPIO_KEY3,
* GPIO_KEY4,
* GPIO_REL1,           // Relays
* GPIO_REL2,
* GPIO_REL3,
* GPIO_REL4,
* GPIO_REL1_INV,
* GPIO_REL2_INV,
* GPIO_REL3_INV,
* GPIO_REL4_INV,
* GPIO_LED1,           // Leds
* GPIO_LED2,
* GPIO_LED3,
* GPIO_LED4,
* GPIO_LED1_INV,
* GPIO_LED2_INV,
* GPIO_LED3_INV,
* GPIO_LED4_INV,
* GPIO_PWM0,           // Cold
* GPIO_PWM1,           // Warm
* GPIO_PWM2,           // Red (swapped with Blue from original)
* GPIO_PWM3,           // Green
* GPIO_PWM4,           // Blue (swapped with Red from original)
* GPIO_RXD,            // Serial interface
* GPIO_TXD,            // Serial interface
* GPIO_HLW_SEL,        // HLW8012 Sel output (Sonoff Pow)
* GPIO_HLW_CF1,        // HLW8012 CF1 voltage / current (Sonoff Pow)
* GPIO_HLW_CF,         // HLW8012 CF power (Sonoff Pow)
* GPIO_USER,           // User configurable

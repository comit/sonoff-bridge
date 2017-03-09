Module types are defined in sonoff_template.h

This includes a "User Test" module type that you can experiment with

You can add an additional type by editing both the mytmplt and module_t arrays to add your own module type (make sure you keep the order of entries in the two arrays the same)

The module definition is a list that contains the module name, followed by a definition of what each of the 17 GPIO pins on the ESP-8266 are defined to be. Each item in the list can be either 0 (unused), or a pin definition from the list below.

As of 3/9/17 (4.0.3) these functions are:

*   GPIO_PWM1,           // Warm
*   GPIO_PWM2,           // Red (swapped with Blue from original)
*   GPIO_PWM3,           // Green
*   GPIO_PWM4,           // Blue (swapped with Red from original)
*   GPIO_RXD,            // Serial interface
*   GPIO_TXD,            // Serial interface
*   GPIO_HLW_SEL,        // HLW8012 Sel output (Sonoff Pow)
*   GPIO_HLW_CF1,        // HLW8012 CF1 voltage / current (Sonoff Pow)
*   GPIO_HLW_CF,         // HLW8012 CF power (Sonoff Pow)
*   GPIO_USER,           // User configurable
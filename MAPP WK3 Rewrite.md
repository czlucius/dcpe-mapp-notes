# Interfacing: IO Ports and Devices
This chapter will explain the interfacing of I/O ports and devices.

## What?

Many devices can be connected to a microcontroller (LEDs, switches to name a few, but there are much more)

Sensors and actuators alike can be connected; the former to sense the environment, the latter to bring about some changes to the environment (may/may not be because of changes in the environment.)

PIC can be interfaced with most electronic circuits.
## But...
Interfacing is the act of connecting a device to a microcontroller such that it is usable.
### Why?
- Different devices may have different voltage/current requirements
- Devices may be high-power appliances and need to be isolated
- Encoders/decoders - reduce number of IO pins needed.

## Some examples...
### LED Bar
- several (usually 10 LEDs) in a package
- note that anodes are +, cathodes are - (not the other way around as in electrolysis/[galvanic cells](https://en.wikipedia.org/wiki/Galvanic_cell))
	- one way to remember it: A+ for <u>a</u>node <u>+</u>ve
- anodes are driven via current limiting resistors by the PIC output pins.
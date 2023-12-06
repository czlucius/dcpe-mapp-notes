# Chapter 2: An overview of PIC18F4550

## About
The PIC18F4550 is a micro-controlller similar to the Atmel AVR, ARM M series.  
It is programmable with a USB cable via a pre-flashed bootloader: a desktop application is used to transfer the binaries.  
The IDE, C compiler, and downloader program are provided.

A microcontroller is a small computer consisting of CPU, clock, oscilator and memory in a package.

## Key features
The PIC18F4550 has the following characteristics:
- **Flash** memory type
- **32 kB** program memory
- **12 MIPS** CPU
- **2048 bytes** RAM
- **256 bytes** EEPROM
- It supports the following protocols:
	- USART
	- MSSP SPI/I2C
- 1 8 bit timer
- 3 16 bit timers
Therefore **3+1** timers!!!
- 13 channels of ADC, 10 bit
- 2 Comparators (voltage level reference, -> PEEE)
- USB 2.0
- 2 - 5.5 Volts DC

## Form factors
- 28 Leg DIP -> Skinny DIP
- 40 leg DIP -> DIP
- SOIC (Small Outline IC) <sub>Skinny Onsurface IC</sub>: SMD but 28 leg and arrange in DIP format
- TQFP (Thin Quad): 44 Lead square, 11 leg each side
- QFN (Quad Flat): No leads, same as TQFP.


![PIC variants](https://i.imgur.com/5YPnr7y.png)


## Pin Diagram
Most pins can support multiple functions, which can be enabled/disabled via C code.
e.g. Pin 33: RB0, AN12, INT0, FLT0, SDI, SDA
![pin diagram](https://i.imgur.com/PUw5kMf.png)


## A basic circuit
- A 5V supply is needed to connect to pins 11, 32 (Vdd and Vdd1) and GND for pins 12 and 31 (Vss and Vss1)
	- ![image](https://i.imgur.com/VZTERQ4.png)  ![image](https://i.imgur.com/lA0MFyw.png)
	- 100 nF capacitor connected across ground

- Crystal Oscillator required at 20MHz (+ 22 pF caps) at pins 13 & 14
	- ![image](https://i.imgur.com/vp2s8OY.png)
- Reset circuit at pin 1 (MCLR) with resistors 
	- ![image](https://i.imgur.com/lDZAXPp.png)
- Pins 18, 23, 24, 37 for USB downloading
- I/O ports to interface devices.

## Flowcharts
Flowcharts describe the flow of the program from one event to the next.

Start/End  
![Conditional](https://i.imgur.com/yqqWq8C.png)  
Input/Output  
![io](https://i.imgur.com/pNmvuqW.png)
dProcess  
![process](https://i.imgur.com/JDvsUik.png)
Decision/condition  
![decision](https://i.imgur.com/FSOuQ58.png)


## A note on circuit diagrams 
- **Always** add current limiting resistors, 470 ohms.
- It may be connected between 5V and GND. So use **voltage dividers**.
## Review

1. What would be your key considerations in choosing a micro-controller for project?
2. Based on the PIC18F4550 pinout diagram in Figure 2.2 in the notes, answer the following questions

|Q|A|
|-|-|
|At which pins should the power supply (5 volts & ground) be connected?|11, 32 and 12, 31|
|At which pins should the crystal/oscillator be connected to supply the clock to the PIC?|13, 14|
|At which pins should the reset button be connected?|1, MCLR|
3. List what are the differences between Digital Input circuits that use Pull Up vs Pull Down resistors.
	Pull up resistors connect to VCC while pull down resistors connect to GND.
4. What are the differences between parallel communication and serial            
communication?
	Parallel communication is all bits at a time and serial is 1 bit at a time.
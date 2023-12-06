## IO Interfacing
- PIC18F4550 can interface with TTL/CMOS digital devices.

### Reasons for interfacing
#1:
> [!NOTE]
> Most electronic devices CANNOT be directly connected to the I/O pins  
> They have different electrical properties and connecting them directly may not work or **may even damage the micro-controller**.
> - e.g. connecting a 15V motor to digital output
> - connecting a 12V device to digital input
- An interfacing circuit must be placed between microcontroller pin and the high power devices
	- e.g. an NPN transistor
- max current 200 mA (all pin) / 25 mA (1 pin)

#2:
- Due to limited pins, an encoder/decoder is sometimes used, to reduce the number of pins needed 


- effector vs actuator?
## 7 Segment
Multiple 7 segments:
- U1 2981 Chip (driver)
	- like an NPN transistor
	- Outputs the same, but because the output HIGH from uC have too little current.
	- So it helps with the current like an NPN transistor.
- U2 2803 8 inverters
	- like NOT gates (and IED's chip)

- Multiplex between 4 7 segments
	- e.g. display 7 on D4:
	- A, B, C. 0b00000111
	- 
	- RB for select
	- PORTD=0b00000111

```c
TRISD = 0b00000000;
PORTD = 0b10010010;
```

- Common Anode
	- In order to turn a cell on, use logic LOW.

- Multiple 7 segments at once:
	- Persistence of Vision
	- Flash the 7 segments with a delay so fast that your eyes are tricked into thinking they are displaying at the same time.
	- e.g. 4 ms refresh.
![](https://i.imgur.com/qjNSgo3.png)


```c
while (1) // a “infinite loop” to show each digit in turn, repeatedly
{
// enable Digit 2 and display “1 with decimal point on” on it:
PORTB = 0 b 0 0 0 0 0 1 0 0; // enable Digit 2
PORTD = 0 b 1 0 0 0 0 1 1 0; // show 1. on it
delay_ms(4); // introduce a brief delay
// enable Digit 1 and display “2” on it
PORTB = 0 b 00000010; // enable Digit 1
PORTD = 0 b 01011011; // show 2 on it
delay_ms(4); // introduce a brief delay

// enable Digit 0 and display “3” on it
PORTB = 0 b 0 0 0 0 0 0 0 1; // enable Digit 0
PORTD = 0 b 0 1 0 0 1 1 1 1; // show 3 on it
Delay... // introduce a brief delay
}
```



-  7447 Binary -> 7 Segment decoder
	- A binary to 7447 segment decoder chip is used.

![](https://i.imgur.com/3HcSaJx.png)


![](https://i.imgur.com/WKzzcWw.png)


## Matrix Keypad
![](https://i.imgur.com/YguXHIb.png)
- 4x4 matrix keypad: 16 buttons arranged in a matrix.
- Only 8 PIC pins (4 out, 4 in) needed
- For each row, can only scan 1 column at a time.
- Logic 1 written to only 1 column, 0 to the rest.
- Rows are read, and the row that reads Logic 1 is the pressed button.

### 74C922 Encoder
The 74C922 encoder is used to internally manage these states.
It uses this table:
```
0123
4567
891011
12131415
```

So you would read the pressed button via ABCD, which are connected to RB3-RB0.
RB5 is connected to DA, which goes to Logic HIGH when pressed. **An interrupt can be configured for this**.

## LCD
- Numbers and alphabets displayed in rows of 16 characters.
- In 4 bit mode of LCD, LCD recognises a byte as 2 nibbles (2\*4bits)  
![](https://i.imgur.com/RxpQwa1.png)

## Transistor as switch
- 2N2222
- Use transistor as a switch
- PIC output Logic 1: transistor is ON
	- output Logic 0: transistor is OFF
- PWM:
	- By controlling the duty cycle & frequency of the wave, the pitch & loudness can be controlled.

![](https://i.imgur.com/4vzJ2d9.png)


### DC Motor
A shunt diode is used to allow current to flow for a short while else the motor circuit will be damaged.

A rectangular wave can be produced at the PIC output pin to control the motor
speed. When the duty cycle is high (\*), the motor moves faster. Again, we will do
this (so called Pulse Width Modulation or PWM) in the Timer topic.


## Mechanical Relay
Use a relay (link to Sec 4 Physics) to control high power devices
e.g. 24V, 230V AC
Transistor & relay HIGH: switch for high power circuit closed.
Provide a means of isolation so will not come into contact with one another.
![image](https://i.imgur.com/nYo0v9D.png)


## I/O Ports
- 5 I/O ports: A to E
	- Note E is a 4 bit port which is not really used,
	- and may be used for other functions.
- Many pins have alternate functions
	- e.g. RA0 is also AN0 i.e. an analogue input.
	- In general, when a peripheral is enabled, that pin may not be used as a general purpose I/O pin.

### A
> [!NOTE]
> **ADCON1!!!**
> **PORT A Is a 7 bit port, not 8 bit**!



Available Port A pins are RA5-0. RA6 is connected to OSC2, Oscillator and not available.
> Remember an external oscillator is needed!

Upon reset,
RA5 and RA3-0 are configured as analogue inputs and
RA4 is configured as a digital input. 


> [!NOTE]
> Use ADCON1 for Analog/Digital switching.


<s><pre> Not 8 select!     76543210</pre></s>Use ADCON1 =         0b1111;
You can see below that analog of 12 pins is controlled with just 4 bits. But you cannot select a specific pin to turn A/D, its according to the table.
![PIC Analog](https://i.imgur.com/Wx14jDs.png)




## B
<h3>**8 bit** wide bidi</h3>
TRISB as data directional register
Upon power on reset, RB7-0 are configured as digital inputs.

## C
7 bit wide bidi
> [!WARNING]
> 7 BITS as well
> **RC3 not implemented**. 
> Hence it is a 7 bit BIDI port.
> 
> RC4 and RC5 ONLY DIGITAL INPUTS!



Multiplexed with serial communication modules
- Upon reset/startup, RC7-4, 2-0 digital

>[!NOTE]
>Pin 24 (RC5) and Pin 23 (RC4) are used for USB
downloading of hex file into the micro-controller, so RC5-4 are not available.

## D
Port D is a 8-bit wide bidirectional port. TRISD is the “data directional register” for
Port D.
-  Port D is multiplexed with the 
	- Enhanced CCP module
	- Streaming Parallel Port (SPP).
-  Upon power on reset, RD7-0 are configured as digital inputs. ALL as digital IN.
-  On the micro-controller board (see Figure 3.3), the Port D pins RD7-0 are all
available.
- Enhanced

## E
> [!NOTE]
> E is a 4 bit port. Only 3 are usable.
> **RE2-0 as ANALOG IN on startup!!!**
> RE3 is digital IN on startup

- 4 bit BIDI
- RE3 as MCLR, unavailable.
	- **Note even when RE3 is usable, it can only function as input**.
- TRISE: set I/O mode for RE2-0. RE3 input only.
- 

 Port E is a 4-bit bidirectional wide port. TRISE is the “data directional register” for
Port E. However, RE3 does not have TRISE bit associated with it – it can only
function as input.
3⁄4
 Similar to other ports, Port E is multiplexed with other functions.
3⁄4
 Upon power on reset, RE2-0 are configured as analogue inputs, while RE3 is
configured as a digital input (RE3 is only available if Master Clear (/MCLR)
functionality is disabled).
3⁄4
 To turn Port E into a digital I/O port, use the C-language command below.
ADCON1 = 0x0F; // there are other possibilities



## GPIO as I or O
Use TRIS\<port name\> to specify Input or Output mode of pins
e.g. TRISB = 0b00001111;
Will make RB7-4 output, 3-0 input.

0 for output
1 for input

> [!Implementation specific notes]
> - TRISA bit 7 is unimplemented. Port A is 7 bit.
> 


![ADCON](https://i.imgur.com/tkzz8sn.png)

### Read
if ( PORTAbits.RA0 == 1 ) // if switch closed
.... // do something
if ( ( PORTA & 0b00000001 ) == ( 0b00000001 ) ) // if switch closed
.... // do something
(or != 0)


### Write
PORTAbits.RA1 = 1;
PORTA = PORTA | 0x01; // 1 to HIGH
PORTA = PORTA & 0b11111110;// 1 to LOW



## 

# ADC Conversion


# Q:
- why need to -1 in the formula?
	- because largest is 0b1111...1. 
	- So cannot use 1024. Must use 1023.
- How does ADCON2 work and Tad?
	- Tad is the time you use for acquisition. 
- ADCON0 vs 1?
	- because only 1 ADC channel can be used at a time.
	- So we need to select which is used.





## Acquisition
- acquisition of Analog signals
- Increase input impedance so circuit will not affect sensor.
- Acquisition time: time in which analogue input voltage is sampled and held
- V<sub>in</sub> remains unchanged although V<sub>a</sub> varies.

This is an IMPORTANT formula!
$$
Digital result = (Vin - Vref)/(Vref_+ - Vref_-) * (2**10-1)
$$


MINUS ONE!!!!!

- If not, output will be 0
- We use amplifiers to increase the current. Because impedance of sensor is very high: it will be used to charge the capacitor AND voltage drop will be very high (e.g. 1mA current, but sensor impedance 1 Mohm, our b)
	- The triangles are amplifiers


## ADCON0

- CHS3:CHS0.
**NOT THE CHANNEL TO CHG BETWEEN ANALOG/DIGITAL!!!**

- Bit 0:
	- ADON. ADC converter is typically not turned on. Use ADON to turn on. ADC
	- Do not turn off typically.
- Bit 1:
	- Conversion status bit.
	- GO/DONE
	- Use to start the conversion. 
		- Set to 1 to start the conversion
		- Set to 0 to forcibly interrupt the conversion
	- Once you set to 1, conversion will happen. Once conversion finishes, it will turn to 0.
- Bits 5 - 2:
	- Selects the analog channel.
	- Enable them. ADCON1 is for A/D.



Only hv 1 ADC converter, only can convert once at a time. That's why need to select CHS3-CHS0.
Because among the 3 Analog inputs, the 3 cannot convert at the same time.''**
## ADCON1
- PCFG3:PCFG0
	- used to configure how many ADCON pin you want
	- To change between digital/analog.
	- The long table of AAAAAAAAAAAAA-DDDDDDDDDDDDD 

- Bit 3-0:
	- PCFG3-PCFG0 as shown above.
- Bit 4:
	- VCFG0 voltage ref config bit (Vref+ source)
- Bit 5:
	- Voltage ref config bit (Vref + source)
- Bit 7-6 NO-OP.



- ADCON1





Reference voltage/power supply accuracy: 5-10%. If you want to get higher precision, use external reference voltage V<sub>ain</sub>

## ADCON2
- For choosing of external clock signal
- Oscillators
- Internal frequency is 1 MHz
- Select internal/external clock signal
	- Select what frequency if external
- Close for how many T<sub>AD</sub>
- Choose Acquisition Time!
- total 10 bit conversion time is 11 T<sub>AD</sub>
- After conversion finish, MUST WAIT 3 T<sub>AD</sub>!


- Unit of acquisition time is Tad. Acquisition time is the time it takes to acquire and hold the signal.



- Bit 7 
**Specify left justification or right!**
- The rest will be 0.
- ADFM=1: Right Justify
	- 10 bit at the right
	- ADRESL.
	- 
- ADFM=0: Left Justify.

10 bit result will be held in 
- ADRESH (AD Result High) 8 bit.
- ADRESL (AD Result Low) 8 bit.
- Since 10 bits, both will be occupied it just matters which has the 2 bit or 8 bit.


- Bit 6 NO-OP, Don't Care.

- Bit 5 - 3: A/D acquisition time select bits
	- Change Tad.
	- Can only change 

Bit 2-0: 
111 or 011 both same, choose internal oscillator, 1 MHz (for this PIC)
Else, choose the frequency.
NOTE: you can only choose Fosc/64, 32, 16, 8, 4, 2.


- Acquisition time: 4 Tad.

# Tutorial
[Tutorial PDF](MAPP_Tutorial_4.pdf)
Q4 C Code qn:
**DO NOT use SHIFT! Because 8 bits, will make result 0. Use \*256. **
```c
int main(void) {
	ADCON0 = 0b00/**/0000/**/01;
	ADCON1 = 0b00/**/0/**/0/**/1110;// Vref+ = Vdd, Vref- = Vss. AN0 As analog.
	ADCON2 = 0b10010110; // 

	ADCON0bits.GO = 1;
	while (ADCON0bits.GO == 1); // Wait until conversion finish.
	while(1) {
		if () {
			PORTB = 0b00000011;
		} else if () {
			PORTB = 0
		}
	}
}
```



# Lab

![i1](Pasted%20image%2020231121105042.png)
- 0b


Q3. 
```c
0b0
```





## Notes from SRQ
Critical for ADC:
- Sampling frequency
- Span of amplitude
- Number of bits (will det. quant. err)

The purpose of the capacitor is to **HOLD** the charge for the acquisition time so it can be captured. NOT to eliminate noise!!!!

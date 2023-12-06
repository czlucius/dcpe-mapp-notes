# Chapter 4
This is Chapter 4, on Analog->Digital converters.
This document will be split into different subsections.

## Introduction 
- microcontrollers can only work with digital data
- outside world is analogue
- we use transducers to convert physical quantities into electrical quantities BUT they are still analogue.
- A->D converts analog signals to digital signals
- PIC has an A->D converter but D->A still required.

## Characteristics
An ADC conversion has several characteristics in its calculation.
1. Acquisition time
 Signals need to be held for a certain time before they are acquired.
![](https://i.imgur.com/eVaV9ap.png)
> Why can't we directly connect to the A->D converter itself?
> - Because the impedance will be too high, and the current too low
> - The voltage reading will show a constant 0 (LOW).

- The capacitor is used to "sample and hold" the voltage.
- The op-amp is to amplify the signals.
- After the switch is opened, V<sub>in</sub> holds the former value of Va, and is now independent of Va.

This time is $T_{AD}$. Sample I found scans SMB after dropping WannaCrypt. Can anyone confirm it’s the same thing? P2P spreading ransomware would be significant. [pic.twitter.com/zs5Td4ovvL](https://t.co/zs5Td4ovvL)

2. Conversion
Vin is converted/quantised into  10 bit digital equivalent.

$\frac{Vin - Vref_{-}} {V_{ref+} - V_{ref-}} * [2^{10} - 1]$

2.1. Note on quantisation error:
Quantisation error happens because the discrete digital signals is not enough to hold infinite precision of the analogue input.

This error is calculated, and DIVIDE BY 2 is required!
Because quantisation rounds the value
> e.g. if steps are 0 and 0.2 V (0 and 1)
> If V is 0.0999V, it will be 0 but after 0.1 it will be quantised as 1
> Therefore quantisation error is 5/1023/2 (both sides.)


## Structure



## Usage
To use the ADC:
- The ADC can only be used one channel at a time.
- The ADC requires you to select which inputs you need to be analog, (if not they will be digital pins)


C code to read a value from AN1:
```c
ADCON0 = 0b00000101 ;// an1, enable, don't start 1st.
ADCON1 = 0b00001110 ;
ADCON2.ADFM = 1; // right justify

ADCON0bits.GO = 1;
while (ADCON0bits.GO == 1); // wait for done

// The result will be stored in ADRESH and ADRESL
// Since we have right justified:
int result = 0;
result |= ADRESL;
result |= (ADRESH << 8);
// result will be the data!
```


## Structure
- The PIC18F4550 has 13 input channels.
- However, only 1 can be used at once for conversion.
- Result in both `ADRESH` and `ADRESL`.

## ADCO
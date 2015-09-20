# Project Page for Designing a Class D Amplifier
<!--- 
(Note: This is a project of physics students quite unfamiliar with
electronics, so there is no guarantee for the quality of the argumentation
presented.) 
-->

## Overview


## Implementation

General points:
  * The high-frequency parts should be as closely together as possible
    (more on PCB Design [here](http://www.eetimes.com/document.asp?doc_id=1274882)).
  * AD modulation [3.1 here](http://www.ti.com/lit/an/sloa119b/sloa119b.pdf).
  * Full-Bridge topology[Fig 5b here](http://www.coldamp.com/store/media/pdf/Class_D_audio_amplifiers_White_Paper_en.pdf)

### Input Stage
Input is inserted to the circuit with a 3.5 mm mono jack plug that usually has
three pins (so that unplugged states can be detected). 

An active low-pass filter of 2nd/3rd order 
([Butterworth coefficients and Sallen-Key topology](http://www.ti.com/lit/an/sloa049b/sloa049b.pdf)) 
with a corner frequency of around 30 kHz to 40 kHz shall filter input device noise
while not compromising the input signal in the audible 20 Hz to 20 kHz range.
This removes unwanted high frequency components that may deteriorate the
modulation (if the noise is close to or higher than the modulation
frequency) and produce electromagnetic inference throughout the circuit.

After low-pass filtering, the signal is ac-coupled to the
modulator/comparator using a simple capacity of around 5 μF. This assures
that the input to the comparator is centered around 0 V as will be the
input from the triangle generator. Frequencies of 20 Hz and above are
barely affected by this (Question: How can we use ac coupling when the
input of the comparator has low currents?).

### Triangle/Sawtooth Generator
For the pulse width modulation a triangle or sawtooth signal of a few
hundred kHz is needed. The shape of the triangle/sawtooth is of high
importance, as non-linearities will lead to systematic distortions of the
signal, independent of the modulation frequency. (Put precisely though, the
flanks don't have to be linear if the frequency is high enough; rather the
duty must be proportional to the height.)

...

Since the audio input and the triangle/sawtooth voltage should be centered
around the same potential, the generator is expected to output a signal
without dc component.

### Modulator
The filtered audio input V1 and the generated triangle V2 are compared to
yield the pulse width modulated signal Vm (high if V1 > V2, low otherwise)
and its inverse Vmi. For this task the comparator lm360 is used.

### Output Stage
Design questions:
  * [BTL output or single-ended](http://www.eetimes.com/document.asp?doc_id=1274877&page_number=2).

The output filter should be adjusted for high currents, so it must have a small
resistance to prevent power loss. Therefore an LC
([common mode](http://www.eetimes.com/document.asp?doc_id=1274877&page_number=3)/
[hybrid](http://www.eetimes.com/document.asp?doc_id=1274877&page_number=4))
topology with Butterworth coefficients is used (second order, passive). See
values for different speaker impedances [here](http://www.ti.com/lit/an/sloa119b/sloa119b.pdf).
Points regarding the [Material Selection](http://www.eetimes.com/document.asp?doc_id=1274877):
  * Pay attention to DC current rating of L; low change in inductance load
    current (max 10%).
  * Use LC output filter without ceramic capacity.


## Sources
  * [Active Low-Pass Filter Design](http://www.ti.com/lit/an/sloa049b/sloa049b.pdf)
  * [Class-D LC Filter Design](http://www.ti.com/lit/an/sloa119b/sloa119b.pdf)
  * [Understanding output filters for Class-D amplifiers](http://www.eetimes.com/document.asp?doc_id=1274877)
  * [Circuit board layout guidelines for Class-D amplifiers](http://www.eetimes.com/document.asp?doc_id=1274882)
  * [Class D audio amplifiers: theory and design](http://www.coldamp.com/store/media/pdf/Class_D_audio_amplifiers_White_Paper_en.pdf)
  * [Class D Audio Amplifier Basics](http://www.irf.com/technical-info/appnotes/an-1071.pdf)
  * [Class D Audio Amplifiers: What, Why, and How](http://www.analog.com/library/analogDialogue/archives/40-06/class_d.pdf)

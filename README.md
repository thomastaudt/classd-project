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
    (more on PCB design [here](http://www.eetimes.com/document.asp?doc_id=1274882)).
  * AD modulation ([3.1 here](http://www.ti.com/lit/an/sloa119b/sloa119b.pdf)).
  * Full-Bridge topology ([Fig 5b here](http://www.coldamp.com/store/media/pdf/Class_D_audio_amplifiers_White_Paper_en.pdf)).

### Input Stage
Input is inserted to the circuit with a 3.5 mm mono jack plug:

![Input Stage Circuit](https://rawgit.com/thomastaudt/classd-project/master/circuits/input_stage.svg)

An active low-pass filter of 2nd 
([Butterworth coefficients and Sallen-Key topology](http://www.ti.com/lit/an/sloa049b/sloa049b.pdf), this [page](http://sim.okawa-denshi.jp/en/OPseikiLowkeisan.htm) is helpful) 
with a corner frequency of around 30 kHz shall filter input device noise
while not compromising the input signal in the audible 20 Hz to 20 kHz range.
This removes unwanted high frequency components that may deteriorate the
modulation (if the noise is close to or higher than the modulation
frequency) and produce electromagnetic inference throughout the circuit.
The supply voltage for the NE5532 is +6V and -6V (as for the rest of the
circuit).

After low-pass filtering (and amplifying with a gain of 2), the signal is
ac-coupled to the
modulator/comparator using a simple capacity of 5 μF. This assures
that the input to the comparator is centered around 0 V as will be the
input from the triangle generator. Frequencies of 20 Hz and above are
barely affected by this.

### Triangle/Sawtooth Generator
For the pulse width modulation a triangle or sawtooth signal of usually 100kHz
to 500 kHz is used. The shape of the triangle/sawtooth is of high
importance, as non-linearities will lead to systematic distortions of the
audio signal, independent of the modulation frequency. (Put precisely though,
the flanks don't have to be linear if the frequency is high enough, but the
duty must be proportional to the height.)

For this project, a simple opamp-driven multivibrator circuit was used:

** COMING SOON **

This way, formidable triangle waves of frequencies up to 400k could be
produced. Note that the usage of the high performance opamp CA3130 was
necessary in order to archieve this frequencies (an LM358 failed for much lower frequencies while an NE5532 did better but failed at producing sharp peaks).

The resistors and the capacity were chosen such that a 125kHz triangle with
an amplitude of about 1V resulted. The frequency was relatively small, as a
faster modulation would have led to problems in the remaining circuit, which
is not well adepted for too high frequencies.

Since the amplitude of the modulation signal in relation to the audio input
determines, how strong the actual amplification of the class D amplifier will
be, a non-inverting opamp-amplifier (NE5532) is used on the original triangle.
The amplitude of the triangle may then be adjusted with an potentiometer
governing R1.

Remark: The triangle generated here is actually part of the charging curve
of the 5μF film capacitor, but an early cutoff makes it very triangluar-ish.


### Modulator
The filtered audio input V1 and the generated triangle V2 are compared to
yield the pulse width modulated (PWM) signal Q (high if V1 > V2, low otherwise)
and its inverse Qi. For this task the comparator lm311 is used.

### Output Stage
The most interesting part of the class D amplifier is the output stage, where
(1) the actual amplification happens and (2) the signal is low-pass-filtered to (at least partially) remove the high-frequency components of the modulated
signal. Two designs are common: Half-bridge or full-bridge (see e.g.
[here](http://www.eetimes.com/document.asp?doc_id=1274877&page_number=2)).



Design questions:
  * [BTL output (full-bridge) or single-ended (half-bridge)](http://www.eetimes.com/document.asp?doc_id=1274877&page_number=2).
  * MOSFET gate drivers / dead-time generation

The output filter should be adjusted for high currents, so it must have a small
resistance to prevent power loss. Therefore an LC
([common mode](http://www.eetimes.com/document.asp?doc_id=1274877&page_number=3)/
[hybrid](http://www.eetimes.com/document.asp?doc_id=1274877&page_number=4))
topology with Butterworth coefficients is used (second order, passive).
Filter designs for different speaker impedances can be found
[here](http://www.ti.com/lit/an/sloa119b/sloa119b.pdf).
Points regarding the [Material Selection](http://www.eetimes.com/document.asp?doc_id=1274877):
  * Pay attention to DC current rating of L.
  * Use LC output filter without ceramic capacity.


## Sources
  * [Active Low-Pass Filter Design](http://www.ti.com/lit/an/sloa049b/sloa049b.pdf)
  * [Class-D LC Filter Design](http://www.ti.com/lit/an/sloa119b/sloa119b.pdf)
  * [Understanding output filters for Class-D amplifiers](http://www.eetimes.com/document.asp?doc_id=1274877)
  * [Circuit board layout guidelines for Class-D amplifiers](http://www.eetimes.com/document.asp?doc_id=1274882)
  * [Class D audio amplifiers: theory and design](http://www.coldamp.com/store/media/pdf/Class_D_audio_amplifiers_White_Paper_en.pdf)
  * [Class D Audio Amplifier Basics](http://www.irf.com/technical-info/appnotes/an-1071.pdf)
  * [Class D Audio Amplifiers: What, Why, and How](http://www.analog.com/library/analogDialogue/archives/40-06/class_d.pdf)
  * [Design and analysis of a basic class D amplifier](http://www.eetimes.com/document.asp?doc_id=1274753)
  * [The Class-D Amplifier](http://users.ece.gatech.edu/~mleach/ece4435/f01/ClassD2.pdf)
  * [Design Considerations for Class-D Audio Power Amplifiers](http://www.ti.com/lit/an/sloa031/sloa031.pdf)
  * [Sallen-Key Low-pass Filter Design Tool](http://sim.okawa-denshi.jp/en/OPseikiLowkeisan.htm)

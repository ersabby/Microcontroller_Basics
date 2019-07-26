# ADC
The usual method of bringing analog inputs into a microprocessor is to use an analog-to-digital converter (ADC). Here are some tips for selecting such a part and calibrating it to fit your needs.

In analog-to-digital converter (ADC) accepts an analog input-a voltage or a current-and converts it to a digital value that can be read by a microprocessor.

![Fig1 SimpleADC](https://user-images.githubusercontent.com/51851040/61939756-99727c80-afb1-11e9-87fc-03e2515706a8.gif)

    Figure 1 shows a simple voltage-input ADC.
This hypothetical part has two inputs: a reference and the signal to be measured. It has one output, an 8-bit digital word that represents the input value.

The reference voltage is the maximum value that the ADC can convert. Our example 8-bit ADC can convert values from 0V to the reference voltage. This voltage range is divided into 256 values, or steps. The size of the step is given by:

Vref/256

where Vref is the reference voltage. The step size of the converter defines the converter's resolution. For a 5V reference, the step size is:

5V/256 = 0.0195V or 19.5mV

Our 8-bit converter represents the analog input as a digital word. The most significant bit of this word indicates whether the input voltage is greater than half the reference (2.5V, with a 5V reference). Each succeeding bit represents half the range of the previous bit.

Table 1 Example conversion, on an 8-bit ADC
Bit:	Bit 7	Bit 6	Bit 5	Bit 4	Bit 3	Bit 2	Bit 1	Bit 0
Volts:	2.5	1.25	0.625	0.3125	0.156	0.078	0.039	0.0195
Output Value:	0	0	1	0	1	1	0	0
Table 1 illustrates this point. Adding the voltages corresponding to each set bit in 0010 1100, we get:

.625 + .156 + .078 = .859 volts

The resolution of an ADC is determined by the reference input and by the word width. The resolution defines the smallest voltage change that can be measured by the ADC. As mentioned earlier, the resolution is the same as the smallest step size, and can be calculated by dividing the reference voltage by the number of possible conversion values.

For the example we've been using so far, the resolution is 19.5mV. This means that any input voltage below 19.5mV will result in an output of 0. Input voltages between 19.5mV and 39mV will result in an output of 1. Between 39mV and 58.6mV, the output will be 2.

Resolution can be improved by reducing the reference input. Changing that from 5V to 2.5V gives a resolution of 2.5/256, or 9.7mV. However, the maximum voltage that can be measured is now 2.5V instead of 5V.

The only way to increase resolution without reducing the range is to use an ADC with more bits. A 10-bit ADC has 210, or 1,024 possible output codes. So the resolution is 5V/1,024, or 4.88mV; a 12-bit ADC has a 1.22mV resolution for this same reference.

# Types of ADCs

ADCs come in various speeds, use different interfaces, and provide differing degrees of accuracy. The most common types of ADCs are flash, successive approximation, and sigma-delta.

# Flash ADC

The flash ADC is the fastest type available. A flash ADC uses comparators, one per voltage step, and a string of resistors. A 4-bit ADC will have 16 comparators, an 8-bit ADC will have 256 comparators. All of the comparator outputs connect to a block of logic that determines the output based on which comparators are low and which are high.

The conversion speed of the flash ADC is the sum of the comparator delays and the logic delay (the logic delay is usually negligible). Flash ADCs are very fast, but consume enormous amounts of IC real estate. Also, because of the number of comparators required, they tend to be power hogs, drawing significant current. A 10-bit flash ADC may consume half an amp.

A variation on the flash converter is the half-flash, which uses an internal digital-to-analog converter (DAC) and subtraction to reduce the number of internal comparators. Half-flash converters are slower than true flash converters but faster than other types of ADCs. We'll lump them into the flash converter category.

# Successive approximation converter

A successive approximation converter uses a comparator and counting logic to perform a conversion. The first step in the conversion is to see if the input is greater than half the reference voltage. If it is, the most significant bit (MSB) of the output is set. This value is then subtracted from the input, and the result is checked for one quarter of the reference voltage. This process continues until all the output bits have been set or reset. A successive approximation ADC takes as many clock cycles as there are output bits to perform a conversion.

# Sigma-delta

A sigma-delta ADC uses a 1-bit DAC, filtering, and oversampling to achieve very accurate conversions. The conversion accuracy is controlled by the input reference and the input clock rate.

The primary advantage of a sigma-delta converter is high resolution. The flash and successive approximation ADCs use a resistor ladder or resistor string. The problem with these is that the accuracy of the resistors directly affects the accuracy of the conversion result. Although modern ADCs use very precise, laser-trimmed resistor networks, some inaccuracies still persist in the resistor ladders. The sigma-delta converter does not have a resistor ladder but instead takes a number of samples to converge on a result.

The primary disadvantage of the sigma-delta converter is speed. Because the converter works by oversampling the input, the conversion takes many clock cycles. For a given clock rate, the sigma-delta converter is slower than other converter types. Or, to put it another way, for a given conversion rate, the sigma-delta converter requires a faster clock.

Another disadvantage of the sigma-delta converter is the complexity of the digital filter that converts the duty cycle information to a digital output word. The sigma-delta converter has become more commonly available with the ability to add a digital filter or DSP to the IC die.

# ADC comparison

![adcComp](https://user-images.githubusercontent.com/51851040/61937061-a8563080-afab-11e9-8889-c6e5db93078d.gif)

    Figure 2 shows the range of resolutions available for sigma-delta, successive approximation, and flash converters.
The maximum conversion speed for each type is shown as well. As you can see, the speed of available sigma-delta ADCs reaches into the range of the successive approximation ADCs, but is not as fast as even the slowest flash ADCs. What the tables do not show is the tradeoff between speed and accuracy. For instance, while you can get successive approximation ADCs that range from 8 to 16 bits, you won't find the 16-bit version to be the fastest in a given family of parts. The fastest flash ADC won't be the 12-bit part, it will be a 6- or 8-bit part.

These charts are a snapshot of the current state of the technology. As CMOS processes have improved, successive approximation conversion times have moved from tens of microseconds to microseconds. Not all technology improvements affect all types of converters; CMOS process improvements speed up all families of converters, but the ability to put increasingly sophisticated DSP functionality on the ADC chip doesn't improve successive approximation converters. DSP functionality does improve sigma-delta types because it enables better, faster, and more complex filters to be added to the part.

# Sample and hold

ADC operation is straightforward when a DC signal is being converted. But if the input signal varies by more than one least significant bit (LSB) during the conversion time, the ADC will produce an incorrect (or at least inaccurate) result. One way to reduce these errors is to place a low-pass filter ahead of the ADC. The filter parameters are selected to ensure that the ADC input does not change by more than one LSB within a conversion cycle.

Another way to handle changing inputs is to add a sample-and-hold (S/H) circuit ahead of the ADC.
In electronics, a sample and hold circuit is an analog device that samples the voltage of a continuously varying analog signal and holds its value at a constant level for a specified minimum period of time. Sample and hold circuits and related peak detectors are the elementary analog memory devices. 

![Fig3 S H_Circuit](https://user-images.githubusercontent.com/51851040/61940557-50bbc300-afb3-11e9-86dc-2290a202fcb8.gif)

    Figure 3 shows how a sample-and-hold circuit works. The S/H circuit has an analog (solid state) switch with a control input. When the switch is closed, the input signal is connected to the hold capacitor and the output of the buffer follows the input. When the switch is open, the input is disconnected from the capacitor.

The figure shows the waveform for S/H operation. A slowly rising signal is connected to the S/H input. While the control signal is low (sample), the output follows the input. When the control signal goes high (hold), disconnecting the hold capacitor from the input, the output stays at the value the input had when the S/H switched to hold mode. When the switch closes again, the capacitor charges quickly and the output again follows the input. Typically, the S/H will be switched to hold mode just before the ADC conversion starts, and switched back to sample mode after the conversion is complete.

In a perfect world, the hold capacitor would have no leakage and the buffer amplifier would have infinite input impedance, so the output would remain stable forever. In the real world, though, the hold capacitor will leak and the buffer amplifier input impedance is finite, so the output level will slowly drift down toward ground as the capacitor discharges.

The ability of an S/H circuit to maintain the output in hold mode is dependent on the quality of the hold capacitor, the characteristics of the buffer amplifier (primarily input impedance), and the quality of the sample/hold switch (real electronic switches have some leakage when open). The amount of drift exhibited by the output when in hold mode is called the droop rate, and is specified in millivolts per second, millivolts per microsecond, or microvolts per microsecond.

A real S/H circuit also has finite input impedance, because the electronic switch isn't perfect. This means that in sample mode, the hold capacitor is charged through some resistance. This limits the speed with which the S/H can acquire an input. The time that the S/H must remain in sample mode in order to acquire a full-scale input is called the acquisition time, and is specified in nanoseconds or microseconds.

Since some impedance is in series with the hold capacitor when sampling, the effect is the same as a low-pass RC filter. This limits the maximum frequency that the S/H can acquire. This is called the full power bandwidth, and is specified in kilohertz or megahertz.

As mentioned, the electronic switch is imperfect and some of the input signal appears at the output, even in hold mode. This is called feedthrough, and is typically specified in decibels.

The output offset is the voltage difference between the input and the output. S/H circuit datasheets typically show a hold mode offset and sample mode offset in millivolts.

Sample and Hold can also be done using software that will control the charge and discharge time of the capacitor

# Internal microcontroller ADCs

Many microcontrollers contain on-chip ADCs. Typical devices include the Microchip PIC167C7xx family and the Atmel AT90S4434. Most microcontroller ADCs are successive approximation because this gives the best tradeoff between speed and the cost of real estate on the microcontroller die.

The PIC16C7xx microcontrollers contain an 8-bit successive approximation ADC with analog input multiplexers. The microcontrollers in this family have from four to eight channels. Internal registers control which channel is selected, start of conversion, and so on. Once an input is selected, a settling time must elapse to allow the S/H capacitor to charge before the A/D conversion can start. The software must ensure that this delay takes place.

Conversion accuracy

Some microcontrollers, such as the Microchip family, allow you to use one input pin as a reference voltage. This is normally tied to some kind of precision reference. The value read from the A/D converter after a conversion is:

(Vin/Vref) x 256

Some microcontrollers use the supply voltage as a reference. In a 5V system, this means that Vrefis always 5V. So measuring a 3.2V signal with an 8-bit ADC would produce the following result:

(Vin x 256)/Vref

= (3.2v x 256)/5V

= 16310

= A316

However, the result is dependent on the value of the 5V supply. If the supply voltage is high by 1%, it has a value of 5.05V. Now the value of the A/D conversion will be:

(3.2V x 256)/5.05V = 16210 = A216

So a 1% change in the supply voltage causes the conversion result to change by one count. Typical power supplies can vary by 2% or 3%, so power supply variations can have a significant effect on the results. Power supply outputs can frequently vary with loading, temperature, AC input variations, and from one supply to the next.

This brings up an issue that affects all ADC designs: the accuracy of the reference. A typical ADC reference might be nominally 2.5V, but can vary between 2.47V and 2.53V (these values are from the data sheet for a real part). If this is a 10-bit ADC, converting a 2V input at the extremes of the reference ranges gives the following results:

At Vref= 2.47V,

Result = (2V x 1,024)/2.47 = 82910

At Vref= 2.53V,

Result = (2V x 1,024)/2.53 = 80910

The variation in the reference voltage from part to part can result in an output variation of 20 counts.

![Fig4ReferenceVoltVar](https://user-images.githubusercontent.com/51851040/61940209-8f04b280-afb2-11e9-9eb2-9671cc102a68.gif)

		Figure 4 shows the effect a reference variation has on the ADC result.
Although the percentage of error stays the same throughout the range, the numerical error is of course greater for larger ADC values.

Software calibration

Sometimes you need an accurate reference, more accurate than the product cost will support. When manual adjustment is out of the question, the software can compensate for reference voltage variations. This is typically done by providing a known, precise input, which is used to calibrate the ADC. This reference can be very precise (and very expensive) because only a few are needed for the production line.

Ref:https://www.eetimes.com/document.asp?doc_id=1276974#

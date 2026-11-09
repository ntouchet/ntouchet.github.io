---
layout: post
title: Chapter 3 
permalink: /academics/dsp-assignments/chapt3/
tags: [academics, DSPSmith, DSP]
---
\*\* <small>*This page details my solution to Chapter 3 programming assignment from Stephen W. Smiths "The Scientists and Engineers
Guide to Disgital Signal Processing."*</small> \*\*
## CHAPTER 3: ADC and DAC
## Programming Assignment 

#### 1. Generate a 1000 point digital signal that simulates an analog signal, consisting of a sine wave at 30.76 hertz, being sampled at 1 kHz for 1 second.  Plot this digital signal with the x-axis labeled "time", running from 0 to 1 second.  

<img src="/assets/img/dsp_chapt3/analog_signal.svg" width=500>
#### 2. Simulate sampling the analog signal at 200 Hz, by discarding every 4 out of  5 samples from the digital signal you created in step 1.  

##### a. Plot this resampled signal, with the x-axis labeled as "time", running from 0 to 1 second.  
The following image has the decimated signal plotted in orange and the original signal plotted in blue.
<img src="/assets/img/dsp_chapt3/200_hz.svg" width=500>
##### b. Has aliasing occurred in this signal? Explain.  
Aliasing has not occurred in this signal. The frequency information of the signal was preserved through the decimation process.  
##### c. What is the digital frequency of this signal?  
The digital frequency is 30.76 Hz. The sampling 
##### d. Could the original analog signal be reconstructed from this digital signal?  
The original analog signal could be reconstructed from this digital signal.
#### 3. Repeat #2 with the sampling at 50 Hz, 33.33 Hz, and 28.57 Hz.

#### *50 Hz*

##### a. Plot this resampled signal, with the x-axis labeled as "time", running from 0 to 1 second.  
The following image has the decimated signal plotted in orange and the original signal plotted in blue.
<img src="/assets/img/dsp_chapt3/50_hz.svg" width=500>
##### b. Has aliasing occurred in this signal? Explain.  
This signal has aliased. The sampling frequency was set to 50Hz and the frequency of the sine wave is 30.76 Hz which is above half of the sampling frequency. While it is not apparent from the image that the signal is aliased, because I simply drew straight lines between the points and did not try to simulate an actual digital to analog conversion, the frequency tells us that aliasing occurred.
##### c. What is the digital frequency of this signal?  
The digital frequency of the signal is 19.24 Hz. This answer was calculated using the following figure. This can also be gathered by counting the number of peaks or troughs in the image above.
<img src="/assets/img/dsp_chapt3/aliasing.png" width=500>
##### d. Could the original analog signal be reconstructed from this digital signal?  
The original analog signal cannot be reconstructed from this digital signal.

#### *33.33 Hz*

##### a. Plot this resampled signal, with the x-axis labeled as "time", running from 0 to 1 second.  
The following image has the decimated signal plotted in orange and the original signal plotted in blue.
<img src="/assets/img/dsp_chapt3/33_33_hz.svg" width=500>
##### b. Has aliasing occurred in this signal? Explain.  
This signal has aliased. It is quite obvious that the frequency of the digital signal (orange) is not the same as the analog signal (blue). 
##### c. What is the digital frequency of this signal?  
The digital frequency of the signal is 2.57 Hz. This was calculated using the same way as before. The phase shift of this signal is also $180^\circ$.  
##### d. Could the original analog signal be reconstructed from this digital signal?  
The original analog signal cannot be reconstructed from this digital signal.

#### *28.57 Hz*

##### a. Plot this resampled signal, with the x-axis labeled as "time", running from 0 to 1 second.  
The following image has the decimated signal plotted in orange and the original signal plotted in blue.
<img src="/assets/img/dsp_chapt3/28_57_hz.svg" width=500>
##### b. Has aliasing occurred in this signal? Explain.  
This signal has aliased. It is quite obvious that the frequency of the digital signal (orange) is not the same as the analog signal (blue). 
##### c. What is the digital frequency of this signal?  
The digital frequency of the signal is 2.19 Hz. This was calculated using the same way as before. The phase shift of this signal is also \\(0^\circ\\).  As shown by this and the previous example the phase shift follows the pattern below.  

<img src="/assets/img/dsp_chapt3/phase_shift.png" width=500>
##### d. Could the original analog signal be reconstructed from this digital signal?  
The original analog signal cannot be reconstructed from this digital signal.

## Homework

#### 1. Specify how many bits are needed to appropriately digitize each of the following signals.  Choose from: 6 bits, 8 bits, 10 bits, 12 bits, 14 bits, or 16 bits. 

##### a. A signal where the maximum amplitude is 1 volt and the rms noise is 1.5 millivolts.  

The goal in quantizing this signal should be to preserve the signal while adding minimal quantization noise. The quantization noise should be small when compared to the noise present in the signal. If I choose 8 bits that means that 1 volt is 255 and 1.5 millivolts becomes 0.382 times the value of the least significant bit. The quantization noise for a signal with a gaussean quantization noise is equal to 0.29 times the value of the least significant bit. This is on the same scale so I need a larger data size. Now if I try with 10 bits 1 volt correlates with 1024 and 1.5 milivolts is 1.536 times the value of the least significant bit. Now the quantization noise, which is 0.29 times the value of the least significant bit is smaller than the noise already present on the signal, this would be an acceptable word size but not perfect. It would increase the standard deviation of the noise to 1.56 times the value of the least significant bit (the standard deviations add in quadrature). This means that the quantization noise only contributes 1.9% of the amplitude of the total noise on the circuit. 

##### b. A signal with a signal-to-noise ratio of 900 to 1. 

If I quantize with 10 bits the value of the signal takes on 1023, which is the maximum value. If this is the case the amplitude of the noise would be \\(\frac{1}{900}=\frac{NOISE AMPLITUDE}{1023}\\). This give a noise amplitude of 1.137 LSB. If I add the quantization noise 0.29 LSB this becomes \\(\sqrt{(0.29 LSB)^2+(1.137 LSB)^2}=1.174 LSB\\). This means that our new noise ratio is \\(\frac{1023}{1.174}=871.38\\) which is a large decrease in SNR. If I repeat the same math for a 12 bit quantization the SNR gets reduce only to 898.18 and if I use 14 bit it becomes an SNR of 899.89. To keep the SNR relatively the same as the analog signal, I would use a 14 bit quantization. 

I NEED TO REDO USING SNR=RATIO OF POWER and NORMALIZE THE AMPLITUDE
##### c. A signal with a coefficient-of-variation of 0.4%. 

The coefficient of variation of a signal is defined as the ratio of standard deviation to the mean \\(\frac{\sigma}{\mu}\\). This is similar to the SNR but relates to the magnitude of the signal instead of its power, and has the mean on the bottom. In this case the ratio of noise to signal would be 4/1000. If I quantize this at 10 bits the signal takes on a value of 1023 and the noise would take on a value of 4.092 LSB. When I add this noise to the quantization noise which is 0.29 LSB the final noise has a standard deviation of 4.102 LSB. This correlates with a coefficient of variation of 0.401% which is close enough to 4%. 
##### d. A high-fidelity audio system (hint: a jack-hammer is about 50,000 times louder than a pin drop). 


##### e. A black and white digital image (hint: under the best conditions, the human eye can differentiate about 200 shades of gray between pure black and pure white). 



####  2. A scientist evaluates two different digital thermometers.  Each has a digital read-out to the nearest one degree, and updates once each second.  When the temperature is held constant, the digital display of thermometer A does not change, but the display of thermometer B randomly toggles between three to four adjacent readings (i.e., +/- 2 degrees).  For the questions below, assume that the temperature is held constant.  

##### a. What is the largest possible error in an individual reading from A? 

The largest possible error for a reading from thermometer A is +/- half a degree. 

##### b. What is the largest possible error in an individual reading from B?

The largest possible error in a reading from thermometer B is +/- 2.5 degrees. 
##### c. In a single reading, which provides the "best" information?  Explain.   

Thermometer A provides the best information because the precision is higher. Thermometer B seems to have more noise than A but similar accuracy. 
##### d. If 1000 readings are taken with A, what is the standard deviation? 

0.29 degrees is the standard deviation. 

##### e. If 1000 readings are taken with B, what is the approximate standard deviation? (choose from 0.1, 0.5, 2.0 and 4.0).  

Assuming that the noise on thermometer B follows a uniform distribution the standard deviation would be 1.15 degrees. 
##### f. If 1000 readings are taken with thermometer B, what is the "typical error between the mean of the readings, and the mean of the underlying process?  

The typical error for B is $\frac{\sigma}{\sqrt{N}}$ where N is the number of samples. This evaluates to 0.0364 degrees. 
##### g. If 1000 readings are taken with each thermometer, which data set provides the "best" information? Explain.  

The best information would be provided by thermometer A because the error between the calculated mean and mean of the underlying process is smaller for A. 
##### h. How many readings must be taken with thermometer A to reliably detect a temperature change of 0.15 degrees?  (choose from 10, 100, 1000, 1 million, 1 billion, or "it cannot be done").  Explain.  

It cannot be done. Thermometer A is only sensitive to changes in temperature of 0.5 degrees so a change in 0.15 cannot be detected.
##### i. Repeat (h) for thermometer B.  

You can detect a change of 0.15 degrees with an error of +/- .005 degrees with 1 million samples using thermometer B. This is due to dithering, where the additional noise present allows for the mean of the signal to move closer to the underlying value. 


#### 3. An engineer designs a microprocessor controlled ADC board that can acquire an 8 bit sample every 10 microseconds.  His boss walks in and says: "You'll get a raise if the system can be modified to acquire a 12 bit sample every 100 milliseconds- but it needs to be done by tomorrow!"  The first thing the engineer does is to measure the noise on the analog signal entering the ADC chip. He then smiles, and plans how to spend the extra money. In detail, explain what the engineer was looking for in the noise measurement, and how he can make the modification.  

The maximum 8 bit value 254 would become associated with 4095 and 0 would be associated with 0. Each value of the 8 bit quantization would be associated with a value on the 12 bit quantization. This 12 bit signal could be passed through a digital low-pass filter and then decimated to reach a sampling frequency of 100ms. The reason he looked at the noise was to see if its amplitude was large enough to allow for dithering with a Gaussian noise distribution. If the noise makes the quantization levels jump around than you can get increased resolution by oversampling, using a low-pass filter, and then decimating. 


#### 4. An analog electronic signal is composed of three sine waves: 1 kHz @ 1 volt amplitude, 3 kHz @ 2 volts amplitude, and 4 kHz @ 5 volts amplitude (all voltage readings are peak-to-peak).  The signal is digitized with 12 bits, spread over the range of -5 volts to +5 volts.  For each sampling rate below, describe the frequency components that exist in the digital signal. Be sure to specify three things for each component: its digital frequency (a number between 0 and 0.5), its amplitude (in digital numbers, peak-to-peak), and its phase relative to the original analog signal (either 0 degrees or 180 degrees).  

a. Sampling rate = 100 kHz. 
b. Sampling rate = 10 kHz.
c. Sampling rate = 7.5 kHz.
d. Sampling rate = 5.5 kHz.
e. Sampling rate = 5 kHz.
f. Sampling rate = 1.7 kHz.

#### 5. A high-fidelity audio signal (containing frequencies between 20 Hz and 20 kHz) is contaminated with interference from a nearby switching power supply, operating at 32 kHz.  To eliminate the interference, the analog signal is passed through an 8 pole Butterworth filter with a cutoff frequency of 24 kHz. The filtered signal is then digitized at 44 kHz.  

##### a. Sketch and label the frequency spectrum of the analog signal, showing:  the audio frequency band, the interfering signal, the frequency response of the filter, one-half the sampling frequency, and the sampling frequency.   


##### b. What is the approximate attenuation of the filter at the frequency of the interference?  
##### c. What is the effect of the filter on the audio information?   
##### d. Sketch and label the frequency spectrum of the digital signal, showing: the audio frequency band, and the interfering signal.  
##### e. At what frequency does the aliased interference appear in the digitized signal?  
##### f. If an 8 pole Chebyshev filter (6% ripple) were used instead, how would the amplitude of the aliased interference change?  
##### g. Repeat (a)-(f) with the cutoff frequency of the filter set at 20 kHz.   


#### 6. A multirate technique is used to handle the interference in the last problem.  The original analog signal (with interference) is digitized at a sampling rate of 176 kHz.  A low-pass digital filter then removes all passing all  frequencies below 0.12 with less than a 0.02% passband ripple (an easy task  for a digital filter).  The digital signal is then resampled at 44 kHz, that is, every three out of four samples are discarded.  

a. Sketch and label the frequency spectrum of the digital signal before 
filtering, showing: the audio frequency band, the interfering signal, and
the approximate frequency response of the digital filter.
b. Has aliasing occurred during the sampling? Explain.  
c. In a fair comparison, should this digital filter be compared against the 
analog Butterworth filter, or the analog Chebyshev filter? Explain.   
d. How much better is the digital filter performing compared to the analog 
filter you indicated in (c)?  (It can be difficult to compare analog and
digital filters; make some sort of quantitative comparison, and explain your
method).  
e. Sketch and label the frequency spectrum of the digital signal after 
resampling, showing: the audio frequency band, and any interfering signals. 


#### 7. On television, rotating objects such as wagon wheels and airplane propellers often appear to be moving very slowly or even backwards.  This is a result of aliasing, caused by the sampling rate of the video (30 frames per second) being less than twice the frequency of the rotational motion. To understand this, imagine we paint one of the blades of an airplane propeller so that we can identify it from the other blades.  We will then turn the propeller at 33 rotations per second, in a clockwise direction. In frame number 1 of our video sequence, the marked blade happens to be exactly at the top of the propeller.   

a. How many rotations does the marked blade make between two successive
frames?  
b. Draw a sketch of how the propeller would appear in frames 1, 2, 3 and 4.
c. How many frames does it take for the marked blade to again appear at the
top?  
d. What rotational frequency is (c) in rotations per second?   
e. Is this apparent rotation clockwise or counterclockwise? 
f. Explain using Fig. 3-4 how the marked blade's actual frequency, the frame
rate, and the marked blade's observed frequency are related.   
g. Repeat (a) to (f) when the propeller is turning at 57 rotations per
second.


#### 8. When viewed on television at 30 frames per second, what is the apparent rotational rate of a 4 blade propeller, turning at 44.7 rotations per second, if all the blades are identical?  Is the apparent rotation clockwise or counterclockwise?
### References:
 1. Stephen W. Smith. *The Scientists and Engineers Guide to Digital Signal Processing*


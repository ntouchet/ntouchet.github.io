---
layout: post
title: Chapter 2
permalink: /academics/dsp-assignments/chapt2/
tags: [academics, DSPSmith, DSP]
---
\*\* <small>*This page details my solution to Chapter 2 programming assignment from Stephen W. Smiths "The Scientists and Engineers
Guide to Disgital Signal Processing."*</small> \*\*
## CHAPTER 2: STATISTICS, PROBABILITY AND NOISE

Use the following algorithm to find the value of each sample in a test
signal, where the function "rnd" returns a random number between 0 and 1:

```
x = rnd 
if x < 0.6 then x = x-0.2 
```

### 1. Calculation of the histogram.

#### a. Sketch the PDF of the process that generated this signal. 

The function "rnd" would have a uniform distribution for all floats between 0 and 1. Applying the function outlined above creates a left shift of 0.2 for all values below 0.6. The sketch for the PDF for rnd and the signal are shown below.  

<img src="/assets/img/dsp_chapt2/sketch_of_pdf.jpg" width="300">

#### b. Generate and plot a signal containing 300 points from this algorithm. 

 To Plot and generate the signal I created a small app using c and dearImGui. If I were to redo this project I would use python, but I prefer c and wanted to get some experience using dearImGui. A plot of a signal with 300 points is shown below.
<img src="/assets/img/dsp_chapt2/300_point_signal.png" width="500">
#### c. Generate and plot the histogram of 1000 points acquired from this algorithm.  Use a bin width of 0.01. 
<img src="/assets/img/dsp_chapt2/1000_histogram.png" width="500">
#### d. Repeat (b) using 10,000 samples.
<img src="/assets/img/dsp_chapt2/10000_histogram.png" width="500">
#### e. Repeat (b) using 100,000 samples. 
<img src="/assets/img/dsp_chapt2/100000_histogram.png" width="500">
#### f. Estimate the peak-to-peak noise on the histograms in (c) to (e). Give your answer as the actual number, not a percentage.

To calculate the peak to peak noise on the histogram you must calculate its histogram. As stated in the section on mean and standard deviation, the standard deviation is the peak to peak value for non-repetitive a waveform. The noise on the histogram is random, and should not follow any pattern. The standard deviation can be calculated by taking the square root of the variance which is calculated by taking the square root of

$$\sigma^2=\frac{1}{N-1}\left[sum\ of \ squares-\frac{sum^2}{N}\right]$$   

where N is the number of bins, sum of squares is the sum of each bin count squared, and the sum is the total number of samples. This formulation for the variance removes the [[catastrophic cancellation]] that happens when the standard deviation is close to the mean and allows the standard deviation to be easily recalculated by adjusting sums instead of needing to recalculate the mean and standard deviation each time there is a new sample. The peak-to-peak noise on the histograms are as follows (I used different signals than the ones depicted in the images but the noise should be similar because I used the same number of points):
- 1000 points: 5.246 
- 10000 points: 49.49
- 100000 points: 491.2

#### g. Which of these three histograms is the best estimate of the pdf?  The worst estimate?

The histogram with 10000 points is the best estimate of the PDF and the one with 1000 points is the worst estimate. It is clear from the images that the histogram with the largest number of points visually resembles the PDF much more closely. The one with 1000 samples has so much noise that it is difficult to discern that the PDF should be uniformly distributed for the range where is it not equal to zero. 
#### h. Which histogram has the lowest peak-to-peak noise?  The highest peak-to-peak noise?  

The histogram with the lowest peak-to-peak noise is the one with 1000 samples and the one with the highest has 100000 points.
#### i. Explain the apparent contradiction between your answers in (g) and (h).   

There is no contradiction between my answers in part g and part h. The measurement of the peak to peak noise does not relate to the Signal to Noise Ratio (SNR) which is a more meaningful metric when analyzing how effective a given instance of a signal is at representing the underlying process. The one with more points will have a larger peak to peak value because the count in each bin is larger, but the difference between the mean and variance of the signal is very small relative to the difference for the signal with fewer points.
### 2. Calculation of the mean and standard deviation.

#### a. Generate a signal containing 10,000 points from this algorithm. Calculate the mean and standard deviation, using a method of your choice.  

The algorithm that I am using is:

```            
for(int i = 0; i<n_samples;i++){
    domain[i] = (float)i;
    float x = (float)rand_r(&seed)/(float)RAND_MAX;
    if(x < 0.6) x -= 0.2;
    signal[i] = x;
    signal_sum += x;
    signal_sum_of_squares += pow(x,2.0);
    histogram_index = (int)((x-min_range)*100.0);
     //printf("INDEX: %d\n",histogram_index);
    histogram[histogram_index]++;
}
float mean = signal_sum/n_samples;
float standard_deviation = sqrt(1.0/((float)n_samples-1.0)*
...(signal_sum_of_squares-pow(signal_sum,2.0)/(float)n_samples));
```
which is the algorithm outlined in question 1f.

My result for 10000 points.
Mean: 0.381523  
Standard Deviation: 0.373761
#### b. Generate three signals as in (a), and add them together.  Calculate the mean and standard deviation of the resulting signal.
Mean: 1.146185  
Standard Deviation: 0.644953
#### c. Repeat (b) adding 10 signals. 
Mean: 3.793501  
Standard Deviation: 1.182334
#### d. Repeat (b) adding 30 signals.  
Mean: 11.417735  
Standard Deviation: 2.064843
#### e. Make a graph of your data in (a)-(d), proving that when random signals are added, their means add and their standard deviations add in quadrature. In your graphs, the x-axis should be "number of points," and the y-axis should be "mean" or "standard deviation."

The following graphs illustrate that the mean of two independent variables adds linearly and their standard deviation adds in quadrature, meaning that it follows the square root. The equation below shows how the standard deviations add in quadrature.

$$\sqrt{\sigma_1^2+\sigma_2^2+...\sigma_n^2}=\sigma_{signal}$$

<img src="/assets/img/dsp_chapt2/Mean.svg" width="500">

<img src="/assets/img/dsp_chapt2/StandardDeviation.png" width="500">


### 3. The Central Limit Theorem.
  
#### a. Calculate and plot the histogram of the signal in 2(c).   
#### b. On this same graph, show a Gaussian curve with the mean and standard  deviation determined in 2(c).  
 <img src="/assets/img/dsp_chapt2/histogram.svg" width=500>

#### c. Comment on the match between the two in terms of accuracy and precision.  
The Gaussian is a very accurate approximation of the behavior of the signal from 2(c) but the precision is low. The pattern of the signal is accurate to a Gaussian as seen in the previous figure, but there is quite a bit of noise around the curve. To measure the SNR I could calculate the standard deviation of the signal. This would require me to divide the signal into chunks so I could calculate the standard deviation of each chunk with the value of the Gaussian as the mean, and then average the STD of each chunk to get the amplitude of the noise for the whole signal. I would subtract out the signal and divide the square of its amplitude by the variance of the histogram. 
#### d. Would increasing the number of samples improve the accuracy or precision  of the match?  
Increasing the number of samples would not improve the accuracy of the match. This would simply increase the amount of samples in each bin of the histogram.
#### e. Would increasing the number of signals added improve the accuracy of the match? 
This would improve the accuracy of the match by the mean value theorem. 



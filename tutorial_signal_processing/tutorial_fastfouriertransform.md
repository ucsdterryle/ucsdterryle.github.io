# Tutorial on Signal Processing with Fast Fourier Transform

### Keywords: 
Real Fourier Series, Complex Fourier Series, Fourier Transform, Complex Numbers, Euler's Formula, Trignometric Functions, Exponential Functions, Nyquist Frequency/Sampling Rate, Neuroscience, Power Spectrum, Frequency, Period, Alpha Waves, Beta Waves, Gamma Waves, Delta Waves, Electroencephalography, Matplotlib, Python, Signal-to-Noise Ratio, Root Mean Square, Action Potential, Statistics, Standard Deviation, Amplitude, Boxplot, Least Squares, Boxcar, White Noise, Pink Noise, Phase Shift, Continuous Signals, Discrete Signals, Periodic, Non-Periodic, Family-wise Error Rate, Time Domain, Frequency Domain, Zero-Padding, Discrete Fourier Transform, Periodogram, 

## Intoduction:
This is a tutorial created from a neuroscience course assignment processing Electroencephalography (EEG) signals recorded from a CSV file. In order to make sense of this signal as a data set, we will be to process them as waves. Understanding that these signals are periodic and have unique characteristics allows us to explore the different signals embedded from the recordings.

This tutorial will be very technical not from a programming stand point but technical in regards to mathematics and science. Though most of the math and science mostly rely things many would ahve encountered in high school, there are some aspects of this tutorial that will be helped and touch upon more advanced concepts found in college Calculus and Neuroscience courses. These more advanced concepts won't prevent you from being able to complete this tutorial but rather will help give a clearer understanding of the overall tasks. 

This tutorial will also contain information on generating basic plots and statistics.

## Background Information

### Neuroscience
Alpha Waves
Beta Waves
Delta Waves 
Gamma Waves

Action Potential 

## Complex Numbers and Euler's Formula


## Signals and Waves

Nyquist

Signal to Noise Ratio

<img src="https://render.githubusercontent.com/render/math?math=e^{i \pi} = -1">



Before we work with actual EEG data, lets create some plots of signals and waves in Python to get a better understanding of the math and the elements of a periodic signal.

We will need the help of a few libraries: Matplotlib, Numpy

                import numpy as np
                import matplotlib.pyplot as plt

Lets start by first generating a plot of a sine wave.  To do this we will need to use the plot function on Matplotlib.pyplot library. The parameters are the x-values (domain) and the y-values or the values generated from the sine function. We will use the numpy arange function to generate values between 0 and 1 equally spaced by 0.01. The arange function have parameters of the start, end, and increments/spaces between values. 


                # generating values of a sin wave

                # domain/x-values
                x = np.arange(0, 4, 0.01)

                # generate a plot of the sine wave
                plt.plot(x, np.sin(x*np.pi))
                plt.show()
                
                
![Sine Wave Plot](/tutorial_signal_processing/tutorial_sine_wave_1.png)




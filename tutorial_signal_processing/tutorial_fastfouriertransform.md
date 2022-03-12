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


Lets use this opprotunity to work with the Matplotlib.pyplot library and also refamiliarize ourselves with some of the technical aspects of trignometric functions, properties of waves, and transformations we can apply to them.

### Amplitute

The amplitude of a wave is the length measured from the center of the wave to its peak. We can 'stretch' or compress the amplitude of a wave by simply adding a scalar coefficient to the front of the function. We will add a coefficient greater than 1 and anothe that is between 0 and 1 to the sine function and assign different colors to each plot and compare their similarities and differences.

                plt.plot(x, np.sin(x*np.pi), color = 'blue', label='Normal')
                plt.plot(x, 0.5*np.sin(x*np.pi), color='red', label='Shrink')
                plt.plot(x, 2*np.sin(x*np.pi), color='green', label='Stretch')
                plt.plot(x, -2*np.sin(x*np.pi), color='yellow', label='Flip')
                plt.legend()
                plt.show()

![Amplitude Sine Wave Plot](/tutorial_signal_processing/tutorial_sine_wave_2.png)

While this might not be critical, the current settings for displaying plots is fairly small. So to adjust and enlarge the displayed plot lets add the flowing line of code:

                plt.figure(figsize=(10, 8))

                plt.plot(x, np.sin(x*np.pi), color = 'blue', label='Normal')
                plt.plot(x, 0.5*np.sin(x*np.pi), color='red', label='Shrink')
                plt.plot(x, 2*np.sin(x*np.pi), color='green', label='Stretch')
                plt.plot(x, -2*np.sin(x*np.pi), color='yellow', label='Flip')
                
                plt.show()



Notice that the position of the legend is now centered, which we can change. While we are at it we can give the legend a short title to help understand the label descriptors.

                plt.figure(figsize=(10, 8))

                plt.plot(x, np.sin(x*np.pi), color = 'blue', label='Normal')
                plt.plot(x, 0.5*np.sin(x*np.pi), color='red', label='Shrink')
                plt.plot(x, 2*np.sin(x*np.pi), color='green', label='Stretch')
                plt.plot(x, -2*np.sin(x*np.pi), color='yellow', label='Flip')
                plt.legend()
                plt.show()


![Figure Resize Sine Wave Plot](/tutorial_signal_processing/tutorial_sine_wave_3.png)

                plt.figure(figsize=(10, 8))

                plt.plot(x, np.sin(x*np.pi), color = 'blue', label='Normal')
                plt.plot(x, 0.5*np.sin(x*np.pi), color='red', label='Shrink')
                plt.plot(x, 2*np.sin(x*np.pi), color='green', label='Stretch')
                plt.plot(x, -2*np.sin(x*np.pi), color='yellow', label='Flip')
                plt.legend(loc='upper left', title='Amplitude Transformation')
                plt.show()

![Legend Settings Sine Wave Plot](/tutorial_signal_processing/tutorial_sine_wave_4.png)


### Frequency

An important feature of waves and signals are their frequencies. A function or wave's frequency can be understood two ways: (1) he number of times it completes a cycle within a specific period of 'time' or (2) how long it takes for it to complete a cycle. By increasing the frequency of a wave or trignometric function like sine, we see that it flucuates rapidly within the window or compressed horizontally. Decreasing the frequency will make the function flucuate slowly within the window or stretch horizontally. We can manipulate the frequency by adding a scalar multiple tothe input parameter of the sine function. 

                plt.figure(figsize=(10, 8))

                plt.plot(x, np.sin(x*np.pi), color = 'blue', label='Normal')
                plt.plot(x, np.sin(0.5*x*np.pi), color='red', label='Lower Frequency')
                plt.plot(x, np.sin(2.0*x*np.pi), color='green', label='Higher Frequency')
                plt.plot(x, np.sin(5.0*x*np.pi), color='purple', label='Highest Frequency')
                plt.legend(loc='upper left', title='Frequency Transformation')

                plt.show()
                
![Frequency Sine Wave Plot](/tutorial_signal_processing/tutorial_sine_wave_5.png)
                
One thing that isn't too pleasing visually is how the differnt plots are a bit difficult to differentiate between one another. One way to distinguish them further beyond just colors is by add what are called 'markers' that will placed over the plots at certain intervals. We can specify what kind of markers or icons that will be used for each plot which will be reflected in the legend as well. In additon, we can also add a grid to help us view specific values on the window.


                plt.figure(figsize=(10, 8))

                plt.plot(x, np.sin(x*np.pi), color = 'blue', label='Normal', marker='o')
                plt.plot(x, np.sin(0.5*x*np.pi), color='red', label='Lower Frequency', marker='s')
                plt.plot(x, np.sin(2.0*x*np.pi), color='green', label='Higher Frequency', marker='^')
                plt.plot(x, np.sin(5.0*x*np.pi), color='purple', label='Highest Frequency', marker='>')
                plt.legend(loc='upper left', title='Frequency Transformation')
                plt.grid()
                plt.show()
                
![Add Markers Frequency Sine Wave Plot](/tutorial_signal_processing/tutorial_sine_wave_6.png)


                plt.figure(figsize=(10, 8))

                plt.plot(x, np.sin(x*np.pi), color = 'blue', label='Normal', marker='o', markevery=5)
                plt.plot(x, np.sin(0.5*x*np.pi), color='red', label='Lower Frequency', marker='s', markevery=5)
                plt.plot(x, np.sin(2.0*x*np.pi), color='green', label='Higher Frequency', marker='^', markevery=5)
                plt.plot(x, np.sin(5.0*x*np.pi), color='purple', label='Highest Frequency', marker='>', markevery=5)
                plt.legend(loc='upper left', title='Frequency Transformation')
                plt.grid()
                plt.show()


![Marker Settings Frequnecy Sine Wave Plot](/tutorial_signal_processing/tutorial_sine_wave_7.png)

Lets compare the last two plots to see how the markers add a little bit more to understand frequency. Try to notice what this adds to your understanding about frequency and the number of data points generated or collected between signal or functions with low frequency and high frequency. In the beginning our domain was and still is just a list of x-values evenly spaced from 0 to 4 in increments of 0.01, so we have roughly 400 points between 0 to 4.

### Horizontal Phase Shifts

We can manipulate the phase shift of our function/signal by arithmetically adding or subtracting a scalar value to the the input parameter. 

                plt.figure(figsize=(10, 6))

                plt.plot(x, np.sin(x*np.pi), color = 'blue', label='Normal', marker='o', markevery=5)
                plt.plot(x, np.sin(x*np.pi -1), color='red', label='Right Shift', marker='s', markevery=5)
                plt.plot(x, np.sin(x*np.pi + 0.5), color='green', label='Left Shift', marker='^', markevery=5)
                plt.plot(x, np.sin(x*np.pi -np.pi), color='purple', label='Right Shift -pi', marker='>', markevery=5)
                plt.plot(x, np.sin(x*np.pi + 2*np.pi), color='pink', label='Right Shift 2*pi', marker='>', markevery=5)
                plt.legend(loc='upper left', title='Frequency Transformation')
                plt.grid()
                plt.show()
                

![Horizontal Phase Shift Sine Wave Plot](/tutorial_signal_processing/tutorial_sine_wave_8.png)


### Aliasing

From the last example with phase shifts we can see that waves/signals/functions can overlap one another despite being different functions. This idea is called aliasing. Two functions can overlap each other partially or completely depending on their frequencies and phase shifts. This can be particularly troublesome or useful depending on what you are doing. One way this can be 'inconvenient' is that two functions might be present while we might only be able to notice one since the other might be hiding with the other function as a function with a similar frequency. This idea of aliaising is seen in real life when we see recordings of a helicopter's top rotor. Since video recordings are a series of pictures captured at 24 frames (or pictures) per second, if the rotors are spining at a frequency matching the frequency of the recording by a certain mutliple what we might see when watching the recording is the rotors looing still and as if it was not moving or spining at all. This is called aliasing.

### Sampling

Now lets look at the idea of recording an collecting data. Suppose to you trying to record sound or a signal of some sort. Like recording video, recording a sound or signal requires us to capture data at a specific point in time, depending on how often we sample or record a specific moment of data taht can affect what we might notice. The idea is that we can sample or record many moments or specific points in time however that could cost us a lot of memory to hold and process. However, collecting too little data may ignore important information or data we need to understand the signal we are looking at. In addition, how we record data could also lead us to the above issue of aliasing. 

Lets go back to the idea of our domain from the beginning were x were values from 0 to 4 with intervals of size 0.01. Lets imagine this was a normal sampling rate over 4 seconds. A rate where we were sampling 100 times per second. Lets see what happens when we create new domains which higher and lower sampling rates. 

                a = np.arange(0,4, 0.1)
                b = np.arange(0,4, 0.2)
                c = np.arange(0,4, 0.5)

                plt.figure(figsize=(10, 6))

                plt.plot(x, np.sin(x*np.pi), color = 'blue', label='Normal Sampling Rate')
                plt.plot(a, np.sin(a*np.pi - 0.5), color = 'red', label='10x Lower Sampling Rate')
                plt.plot(b, np.sin(b*np.pi - 1.0), color = 'green', label='20x Low Sampling Rate')
                plt.plot(c, np.sin(c*np.pi - 1.5), color = 'purple', label='50x Higher Sanpling Rate')
                plt.legend(loc='upper right', title ='Sampling Rate')
                plt.grid()
                plt.show()


![Sampling Rate Sine Wave Plot](/tutorial_signal_processing/tutorial_sine_wave_9.png)

To measure sampling rate, we use the units of hertz (Hz) that has the units 1/seconds. So in our first example with x being points from 0 to 4 with 0.01 between each point and treating the 4 as 4 seconds we would be sampling 400 points over 4 seconds which is 100 points per second which is 100 Hz. In our most recend example above 'a' would be 10 Hz, 'b' would be 5 Hz, and 'c' would be 2 Hz. 

### Nyquist Limit Sampling Rate



### Complex Numbers



### Angular Frequency



## Fourier Series



## Discrete Fourier Transform


## Power Spectral Decomposition (PSD)


## Autocorrelation


## Filters

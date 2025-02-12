---
layout: default
title: Lab 2
---

# Lab 2

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML">
</script>


## Task 1: Setup
To begin the lab, I installed the nessesary libraries for the IMU and plugged it directly into the Artemis. The ADR jumper was open on the breakout board so ADO-0 was set to 1. I then ran the Example1-Basics tutorial to test my IMU. The results shown below illustrate the IMU's ability to measure acceleration, angular rate, and magnatometer data.

<iframe width="560" height="315" src="https://www.youtube.com/embed/DglJvvwAnV8" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

In addition, to make the IMU easier to work with, I added code that makes the IMU blink 4 times after being initialized by adding the following code at the end of the setup loop.

![image](https://github.com/user-attachments/assets/edcd2b88-fa25-4e2d-ad07-1953e29a6f76)

<iframe width="560" height="315" src="https://www.youtube.com/embed/tuaYMzHnW-0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>



## Task 2: Accelerometer

The accelerometer outputs acceleration in the x,y, and z axis. To convert these accelerations to pitch ($\theta_a$) and roll ($\phi_a$), we can use the formulas below.

<img src="https://github.com/user-attachments/assets/62da5f7d-7d45-4348-9331-ffc00310d61c" width="200">


Or in Arduino code:

![image](https://github.com/user-attachments/assets/7e405ae1-08d2-447f-a01e-71177ee8be9b)

Note that there is a 1.03 factor in front of both equations. This is because after preforming a two point calibration, I found I needed a correction factor of 1.03 on both pitch and roll. Data after the two point calibration for a (0 -> 90) deg and (0 -> -90) degree rotations are shown for both pitch and roll.


Pitch             |  Roll
:-------------------------:|:-------------------------:
![pitch_neg_90](https://github.com/user-attachments/assets/681ed4e3-ad83-4e41-b51b-a56ae92ad447) |  ![roll_90_neg](https://github.com/user-attachments/assets/a8c3c017-fbee-4ee1-9bdf-2f1829a4592f)
![pitch_90](https://github.com/user-attachments/assets/6622548a-b763-418b-83e8-132d3854dfb8) |  ![roll_90_pos](https://github.com/user-attachments/assets/7497d06d-1033-44b7-b887-d2a2ecdfe0e6)






As shown above, the data from the IMU has a fair amount of high frequency noise. To remove this noise, I collected roll and pitch data as I periodically rotated the IMU as shown below.

Pitch data:

![Pitch](https://github.com/user-attachments/assets/bd92a185-ea58-4c74-bff3-d427f9e1d5f1)

Roll data:

![Roll](https://github.com/user-attachments/assets/4c4892c3-bb64-484f-a89b-564cd80b8e8a)

To analyze the noise in this data, I preformed an FFT of the data. As can be seen by the FFT of the pitch data, most of the signal is contained between 0 and 6hz. Therefore, I can build a low pass filter to eliminate frequencies higher than that as they are likely noise.

![Pitch_FFT](https://github.com/user-attachments/assets/c19edd5a-6cc1-4419-ac2f-df36b82bdc94)

Using the following recursive formulas, we can construct a low pass filter:

For pitch (very similar for roll):

$\theta_{LP}[n] = \alpha \theta_{raw} + (1 - \alpha) \theta_{LP} [n - 1]$

$\theta_{LP}[n-1] = \theta_{LP}[n]$

To determine $\alpha$ we can use the formulas below. The data above was collected at a sampling period T = 10ms. Setting $f_c$ to 5hz, we get an $\alpha$ of 0.266.

$\alpha = \frac{T}{T + RC}$

$f_c = \frac{1}{2 \pi RC}$

In Arduino:

![image](https://github.com/user-attachments/assets/0f666821-8a19-4777-964f-dcd496897e20)

The result of the LPF is shown below. As can be seen by the plots, the LPF cuts down a considerable amount of the high frequencty noise. 

![Pitch_lp](https://github.com/user-attachments/assets/1e3c858d-72b7-456c-959e-c78c106eecf2)

![Roll_lp](https://github.com/user-attachments/assets/77518a9a-ea0d-4069-b89f-fe3dd4e17d3f)

## Task 3: Gyroscope

To compute orientation from gyroscope data, we need to integrate the angular rates over time using the formulas below.

$\theta_g = \theta_g + g_y dt$

$\phi_g = \phi_g + g_x dt$

$\psi_g = \psi_g + g_z dt$

In Arduino:

![image](https://github.com/user-attachments/assets/1dc30b1d-103d-4f92-98a9-b84745b89963)

We can then overlay the gyroscope data with raw and filtered accelerometer data. From these plots, we can see that the gyroscope has much less noise even when compared to the filtered accelerometer data. However, it tends to drift over time away from the accelerometer measurements. 

![gyro_drift_pitch](https://github.com/user-attachments/assets/ac6a5c91-309a-484e-b487-859345e341ff)

![gyro_drift_roll](https://github.com/user-attachments/assets/bc1a5554-44da-41b8-8537-43b5a4e95ac9)

To prevent drift, we can implement a complementary filter which fuses the gyroscope and filtered accelerometer data together.

$\theta_{fused} = (1 - \alpha)(\theta_{fused} + g_y dt) + \theta_a \gamma $

$\phi_{fused} = (1 - \alpha)(\phi_{fused} + g_x dt) + \phi_a \gamma $

After some experimentation, I found that $\gamma = 0.8$ produced the best results.

Arduino code:

![image](https://github.com/user-attachments/assets/a211981e-23bd-4925-84bc-1e884775f38b)

As can be seen below, the complimentary filter prevents drift over time in the fused signal. After some experimentation, I determined that its working range contained both positive and negative rotations from about -90 to 90 degrees in both pitch and roll. Beyond that, the data seemed to be a little less reliable.

![fused_roll](https://github.com/user-attachments/assets/c44f4031-6ffd-43b9-bad0-eb32c5652b8a)

![roll_fused](https://github.com/user-attachments/assets/2f087917-2fe3-4827-b6c9-913bb83029fd)

It also does a good job of rejecting random vibrations.

![image](https://github.com/user-attachments/assets/3df5ac82-0452-4071-a03a-2201940545ad)


## Task 4: Sampling Data Fast

To sample data as quickly as possible, I implemented get_imu_data() which encapsulates all of the previous functionality into a function. I then called this function in the main loop whenever data is ready and a data collection flag rec_data was set to true.

Arduino code:

![image](https://github.com/user-attachments/assets/226e3729-ee09-4fca-96dc-9a02ce805f87)

To start and stop/transmit recorded data, I implemented two more commands that can be called over Bluetooth by my computer.

![image](https://github.com/user-attachments/assets/0b2547b1-55ac-437f-9e6c-bfd7b831b21f)

![image](https://github.com/user-attachments/assets/2b8739b6-fbe8-469a-a661-76b8d991136a)

After removing all of the delays, I was able to collect data for over 5s at a sampling rate of 367hz.

![image](https://github.com/user-attachments/assets/7eebe0bc-0919-48d0-898b-8b1f8b22eab9)

![image](https://github.com/user-attachments/assets/e617c478-48ac-4855-b137-f1d12d0f002a)

![image](https://github.com/user-attachments/assets/e5f0cd3d-1009-49c8-b979-3fb9599b160f)

![image](https://github.com/user-attachments/assets/8baec17a-997b-42ca-869a-4da1d7df4376)


To store data, I used 3 float arrays for pitch, roll, and yaw. I selected floats over ints and doubles as they provide more accuracy compared to integers but take up less memory when compared to doubles. Therefore, I think that it is the perfect balance between the two. In all, the Artemis has 384kb of memory or or 33,000 float values. Since I am sampling at 367hz, this equates to about 1.5 min of data. This is not to bad for the Artemis. However, there will likely be other things that need to be stored other than just array data so this value will likely be lower.

## Robot Video

Below is a video of my robot driving around a study lounge. As shown, the robot's motors are very powerful and able to easily flip the car. I cannot wait to turn it into a cool robot. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/TwByh9toyM4" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>







































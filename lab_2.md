---
layout: default
title: Lab 2
---

# Lab 2

## Task 1: Setup
To begin the lab, I installed the nessesary libraries for the IMU and plugged it directly into the Artemis. The ADR jumper was open on the breakout board so ADO-0 was set to 1. I then ran the Example1-Basics tutorial to test my IMU. The results shown below illustrate the IMU's ability to measure acceleration, angular rate, and magnatometer data.

In addition, to make the IMU easier to work with, I added code that makes the IMU blink 4 times after being initialized by adding the following code at the end of the setup loop.

![image](https://github.com/user-attachments/assets/edcd2b88-fa25-4e2d-ad07-1953e29a6f76)

## Task 2: Accelerometer

The accelerometer outputs acceleration in the x,y, and z axis. To convert these accelerations to pitch ($\theta_a$) and roll ($\phi_a$), we can use the formulas below.

$\theta_a = atan2(a_x,a_z)$

$\phi_a = atan2(a_y,a_z)$

Or in Arduino code:

![image](https://github.com/user-attachments/assets/e2710dcc-00e6-4475-8a84-a7d2cb22b399)

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

$\alpha = \fraq{T}{T + RC}$

$f_c = \fraq{1}{2 \pi RC}$

In Arduino:

![image](https://github.com/user-attachments/assets/0f666821-8a19-4777-964f-dcd496897e20)

The result of the LPF is shown below. As can be seen by the plots, the LPF cuts down a considerable amount of the high frequencty noise. 

![Pitch_lp](https://github.com/user-attachments/assets/1e3c858d-72b7-456c-959e-c78c106eecf2)

![Roll_lp](https://github.com/user-attachments/assets/77518a9a-ea0d-4069-b89f-fe3dd4e17d3f)

## Task 3: Gyroscope



























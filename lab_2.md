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

The accelerometer outputs acceleration in the x,y, and z axis. To convert these accelerations to pitch($\theta_a$) and roll($\phi_a), we can use the formulas below.







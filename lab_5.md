---
layout: default
title: Lab 4
---
## Task 1 Sampling Frequency
Before we can implement PID control, we must first configure the TOF sensor. I selected the TOF short mode because it provides the highest possible sampling frequency out of all other modes and the least sensor noise. These qualities are important as the controller must recieve data fast enough to allow the PID controller to react quickly to obstacles and not be too noisy as to cause the derivative term to go haywire. The TOF short mode with a timing budget of 20ms allows us to achieve this. 

![image](https://github.com/user-attachments/assets/05240319-8f96-4494-b6ff-21d1a933eb67)

We can verify this sampling rate by running code very similar to that in lab 3 which samples the TOF sensor as fast as possible.

![tof_sampling_rate](https://github.com/user-attachments/assets/13315dce-bba4-44cf-abe3-0d69fe946848)

After sending the accociated measurements and time stamps over bluetooth, I found that I was able to achieve a sampling rate of 45hz which was close to theoretical sampling rate of 50hz on the datasheet. 

## P Controller
With the TOF sensor properly configured, I began implementing a simple p controller using the function shown below. To calculate a good starting point for kp, I knew that the max range I was recieving from my distance sensor was about 2000mm. I wanted this to correspond to a motor command signal of 1 which indicates that the max value of kp should be around 0.0005. However, this caused significant overshoot and I had to reduce kp to 0.00009 to prevent the car from hitting the wall when trying to stop at a set point 1ft from the wall.

![image](https://github.com/user-attachments/assets/10f8885f-32ad-451c-b7e8-f2f17cb77540)

The controller is integrated into the main loop as shown below.

![main_loop_no_ext](https://github.com/user-attachments/assets/7d952f51-b872-47b3-ba76-0df54ba3a616)

The p controller resulted in the following plots of distance and command signal over time.

![p_distance](https://github.com/user-attachments/assets/3c93a671-8366-410f-8116-e8621fcfe27e)

![p_control_signal](https://github.com/user-attachments/assets/fbd9766e-195d-47a9-983c-89ef04a48a80)











---
layout: default
title: Lab 4
---
## Task 1 Sampling Frequency
Before we can implement PID control, we must first configure the TOF sensor. I selected the TOF short mode because it provides the highest possible sampling frequency out of all other modes and the least sensor noise. These qualities are important as the controller must recieve data fast enough to allow the PID controller to react quickly to obstacles and not be too noisy as to cause the derivative term to go haywire. The TOF short mode with a timing budget of 20ms allows us to achieve this. 

![image](https://github.com/user-attachments/assets/05240319-8f96-4494-b6ff-21d1a933eb67)

We can verify this sampling rate by running code very similar to that in lab 3 which samples the TOF sensor as fast as possible 

![tof_sampling_rate](https://github.com/user-attachments/assets/13315dce-bba4-44cf-abe3-0d69fe946848)

After sending the accociated measurements and time stamps over bluetooth, I found that I was able to achieve a sampling rate of 45hz which was close to theoretical sampling rate of 50hz on the datasheet. 



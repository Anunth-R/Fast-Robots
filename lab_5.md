---
layout: default
title: Lab 4
---
## Sampling Frequency
Before we can implement PID control, we must first configure the TOF sensor. I selected the TOF short mode because it provides the highest possible sampling frequency out of all other modes and the least sensor noise. These qualities are important as the controller must recieve data fast enough to allow the PID controller to react quickly to obstacles and not be too noisy as to cause the derivative term to go haywire. The TOF short mode with a timing budget of 20ms allows us to achieve this. 

![image](https://github.com/user-attachments/assets/05240319-8f96-4494-b6ff-21d1a933eb67)

We can verify this sampling rate by running code very similar to that in lab 3 which samples the TOF sensor as fast as possible.

![tof_sampling_rate](https://github.com/user-attachments/assets/13315dce-bba4-44cf-abe3-0d69fe946848)

After sending the accociated measurements and time stamps over bluetooth, I found that I was able to achieve a sampling rate of 45hz which was close to theoretical sampling rate of 50hz on the datasheet. 

![image](https://github.com/user-attachments/assets/4f8736a4-761a-46f5-88a8-9add37227abb)


## P Controller
With the TOF sensor properly configured, I began implementing a simple p controller using the function shown below. To calculate a good starting point for kp, I knew that the max range I was recieving from my distance sensor was about 2000mm. I wanted this to correspond to a motor command signal of 1 which indicates that the max value of kp should be around 0.0005. However, this caused significant overshoot and I had to reduce kp to 0.00009 to prevent the car from hitting the wall when trying to stop at a set point 1ft from the wall.

![image](https://github.com/user-attachments/assets/10f8885f-32ad-451c-b7e8-f2f17cb77540)

The controller is integrated into the main loop as shown below.

![main_loop_no_ext](https://github.com/user-attachments/assets/7d952f51-b872-47b3-ba76-0df54ba3a616)

The p controller resulted in the following plots of distance and command signal over time.

![p_distance](https://github.com/user-attachments/assets/3c93a671-8366-410f-8116-e8621fcfe27e)

![p_control_signal](https://github.com/user-attachments/assets/fbd9766e-195d-47a9-983c-89ef04a48a80)

## PID Controller
One of the most distinct features of the P controller is the large overshoot. This could cause crashes at higher speeds. To compensate for this, I added a derivative term which will act as a damper to reduce that overshoot. This term corresponds to the velocity of the robot estimated from successive distance measurements multiplied by a weighting factor kd. I also added a very small integral term to remove any small steady state errors in my controller. This term estimates the integral of the errror by adding each new error to a Riemann sum and multiplying it by a weighting factor ki. 

![pid_control_function](https://github.com/user-attachments/assets/4a1628bf-3bd6-49c4-97bb-6ad15453426f)

  One important note is that I found it helpful to express kd and ki in terms of kp for ease of tuning. After tuning the controller, I found that kp = 0.0002 and kd = kp/2 produced a very nice response with minimal overshoot. I also sprinkled in ki = kp/100 just to eliminate any long term steady state errors. The accociated distance and command signal plots are shown below. The orange line corresponds to the setpoint target. 

  ![pid_distance](https://github.com/user-attachments/assets/a4d6a318-d9f8-43ff-992b-fc93a639c6de)

  ![pid_control_signal](https://github.com/user-attachments/assets/d85ed693-f568-43d8-97b2-2748c729bd36)

  ## Data Extrapolation
  One important thing to note is that the PID loop rate is currently tied to the rate that data can be sampled from the TOF sensor. One possible solution to this is to move the PID control function outside of the conditional checking for a new measurement. 

  ![pid_loop_rate](https://github.com/user-attachments/assets/782b7dc8-5b2b-4825-a2ed-a75e661536ad)

  Now the PID loop can run at a much faster rate compared to before simply using the last recorded sensor measurement when no new sensor measurement is available. After transmiting the data from a run over to python and analyzing the time stamps, I found that the new sampling frequency was around 286hz which is over six times faster than the previous loop!

  However, we can do better than just using the last sensor measurement. Instead, we can create an extrapolate function which uses the last two distance measurements to estimate the current velocity of the robot. We can then use that velocity to estimate what the new distance measurement should be even if we have no new data.

  ![image](https://github.com/user-attachments/assets/ebf80b53-ca6b-4423-a190-e4295e9e5007)

  Then in the main loop, we can supply our controller extrapolated sensor measurements if we have at least two previous sensor measurements and no new data is available. (It does not make sense to extrapolate with less than two measurements)

  ![pid_main_loop_ext](https://github.com/user-attachments/assets/a8796880-6e52-411d-a06d-12397d838771)

  We can see the effects of extapolation clearly when we overlay the extrapolated measurements with the TOF sensor measurements.

  ![extrapolated_measured_distance](https://github.com/user-attachments/assets/1a06ada3-7bad-448a-8017-84f29d50fb18)

  As shown above, as the robot's velocity does not change too much, extrapolation estimates the sensor measurements reasonably well. The distance and motor input plots using extrapolation is shown below. 

  ![pid_distance_ext](https://github.com/user-attachments/assets/9800a563-3c30-4ead-91ed-f51cddf6e689)

  ![pid_control_signal_ext](https://github.com/user-attachments/assets/a057335e-b778-4f7c-9afd-dd4798ece6b7)

  The controller with extrapolation preformed similar to before but now running at a much faster rate allowing the controller to respond quicker to a dynamically changing environment. 

  ## Low Pass Filter
  One thing that became very apparent to me was how noisy my control signal had become. This was because the derivative term amplifies high frequency noise from the sensors. To compensate for this, I added a low pass filter on the derivative term with alpha = 0.01.

  ![image](https://github.com/user-attachments/assets/79924165-cc4b-4f0c-9800-c3a1346cdbe6)

  This lead to much smoother distance curve and control inputs into my car as shown below. 

  ![filtered_pid_distance](https://github.com/user-attachments/assets/52cc8aff-4dfe-4dba-b367-82b20ce522af)

  ![filtered_pid_control_signal](https://github.com/user-attachments/assets/77db2efc-f983-4de3-a6d4-4ac7c02841bf)

  ## Windup Protection
  One consequence of including an integral term in my controller is that windup can cause instability in the controller. For example, here is a video below showing my foot pinning my car for 5 seconds before releasing it. The car slammed into the obstacle because the integral term completely saturated the control inputs.

  To prevent this from happening, we can constrain the integrated error so that it can contribute no more that 0.15 to the total overall input signal.

![image](https://github.com/user-attachments/assets/90fa2e63-1293-4862-b9f3-a3027ce9ef2e)

  With windup protection, the car is still able to stop even after being pinned for 5 seconds.

  ## Controller Robustness

  ## Discussion
  I really enjoyed implementing PID control on my robot especially because I am also taking the feedback control class this semester. Therefore, this lab gave me a valuable opportunity to see the theory I had learned in that class applied to a real robotic system. 

  



  





















---
layout: default
title: Lab 7
---

# Estimate Drag and Mass

In order to implement a Kalman filter, we need a linear state space model of our system. Our robot can be treated as a simple second order system with both a linear drag and motor force acting on it. If we construct the robot's state out of its position and velocity, we can model our robot using the state space system shown below. y corresponds to the sensor output of our TOF sensor.

<img src="https://github.com/user-attachments/assets/fea87d18-ba57-4eb6-94c3-93ff7f890dbe" width="200">

This model requires knowledge of the drag force (d) and car mass (m). We can determine these quantities by analyzing our car's response to a step control input. The plot below depicts the car's TOF sensor measurements to a control input of 0.4. this control input was selected as it is in the median range of control inputs supplied to the car and therefore will produce data in the region we most care about. As shown by the plot, the car reaches very close to its steady state velocity by the end of the data collection period. (and then slammed into a pillow thankfully)

![image](https://github.com/user-attachments/assets/5d917f70-9b06-4775-8a35-2fac29a846dc)

![image](https://github.com/user-attachments/assets/ef4482c6-de86-482a-8648-f938d7fc1c01)

Using this data, we can compute the velocity of the car and extract the steady state velocity (vss) and rise time (t_r) corresponding to 90% of the steady state velocity. vss was found to be approximately 2.4 m/s and t_r was 1.27s.  

![image](https://github.com/user-attachments/assets/2cc52e81-1949-4f6e-b4a3-bf16d2ca2538)

Using these parameters, we can estimate m and d using the equations shown below. We can now have a complete linear state space model of our car.

<img src="https://github.com/user-attachments/assets/b252ab59-3ca8-48fc-863e-809c653ab427" width="400">

# Initializing the Kalman Filter

We will define A,B,C to be the following matrices.

![image](https://github.com/user-attachments/assets/f76a9d10-7444-4963-8ef3-3ff783bbfce2)


To implement a Kalman filter, we must first descritize our system. I used dt = 0.02s as that is the rate at which sensor data was collected. 

![image](https://github.com/user-attachments/assets/db6f46f6-d9b8-42c5-838f-18169abca68a)

Next we need to select values for our matrices Q (2x2) and R (1x1) which define the process and sensor noise in our system. We will assume that our variables are uncorrelated which means that we need a varience estimate for the position and velocity of the car as well as the TOF sensor readings. We can get ballpark values for the velocity and positions using the following equations. 

![image](https://github.com/user-attachments/assets/028a3de0-0979-4e24-9792-92d79a20fd45)

However, after some tuning, I found that a varience of 20mm seemed to produce better results. For R, I noticed during the previous labs that the TOF sensor had an uncertainty of around 45mm. Therefore, I selected this value for R.

![image](https://github.com/user-attachments/assets/dfa6062a-51af-49eb-bffb-6636ff91c11a)

The Kalman filter also requires an initial guess as to the state of our robot and an accociated covarience estimate. I set the initial state to the first sensor measurement and initial covariance with 20 and 40 along the diagonal as these seemed like a reasonable initial guess. Over time, the Kalman filter should converge to the true value.

![image](https://github.com/user-attachments/assets/4df7fe81-0809-4883-824e-994436196789)

Finally, we can implement the main Kalman Filter function which takes in all of the nessesary perameters and preforms the predict and update steps.

![image](https://github.com/user-attachments/assets/4214bb7c-d16e-407d-a043-3523803fcd8a)

With the Kalman filter implemented, we can load in raw data collected from lab 5 of the car using its PID controller to stop before hitting a wall. We can then loop through that data and use the Kalman filter to estimate the position of the car.

![image](https://github.com/user-attachments/assets/91100cb2-fc45-4b0c-b5b5-d3919e47bbb4)

Here is a plot of the results. As you can see, the Kalman Filter does an exellent job of smoothly tracking the position of the car without trying to chase indevidual sensor values.

![KFE](https://github.com/user-attachments/assets/6c8c0bfb-692d-4ef6-b969-8da8fd9734b4)

![KFE_zoomed](https://github.com/user-attachments/assets/083cf092-14a3-45d4-ac31-49ce6f15f84f)

# Verifying the Integrity of the Kalman Filter

Argueably more important than just the Kalman FIlter's state estimate is its covariance estimate. A good indication that our filter is working properly is that the true position of the car remains within two standard deviations (95% confidence interval) of the true position of the car. Unfortunately, we do not know the true position of the car but we can use the sensor readings as an approximate estimate. As shown by the plots above, the sensor values are contained within the Kalman filter's 2 sigma estimate bounds (the shaded region) so the filter is likely functioning properly.

# Effects of Kalman Filter Parameters

Q and R play a key role in the preformance of our filter. As mentioned before, Q represents the process noise. If we have a lot of process noise, the Kalman filter will not trust our dynamic model and simply chase our sensor values.

![image](https://github.com/user-attachments/assets/1236cc5c-25a4-4716-ab18-8a81e5ea8d44)

R represents the sensor noise. If we have a lot of sensor noise, the Kalman filter will trust the model more and deviate from the sensor values.

![image](https://github.com/user-attachments/assets/8fc87791-af25-4988-8d3e-c318f0c6b611)

Finally, it is important to have a good initial guess for the robot's state and covarience. The Kalman Filter should still converge to a proper state estimate and covarience even if these parameters are off. However, it might struggle a lot in the beginning if these parameters are wildly off.

# Kalman Filter on the Robot (and optional task)

With, the Kalman filter working properly in simulation, we can now implement it on our car. First, we can define a Kalman Filter function now using Arduino/C syntax. We can set update to either true or false depending on whether sensor data is available.

![image](https://github.com/user-attachments/assets/8c0fe549-0857-4283-9372-542804da9e89)

With the Kalman filter implemented, we can now initialize our Kalman filter perameters to the same as those used in simulation. In retrospect I should have modified dt to be the actual loop rate of the Arduino but the filter worked well nonetheless. 

![image](https://github.com/user-attachments/assets/829cdc1d-accb-4a22-a539-0bd715b90857)

Then, we can integrate our Kalman filter into the main loop. Note that the Kalman filter is running whether or not sensor data is available also completing the optional task as well. One thing to note is that I made sure that the Kalman filter only ran after the second sensor measurement to ensure that the filter could properly be initialzied using the first sensor measurement.

![image](https://github.com/user-attachments/assets/be3f360a-8e83-4ab2-b789-c5c6b7a3c735)

After testing the robot with the Kalman filter, we can obtain the following plots and video.

![KFE_on_robot](https://github.com/user-attachments/assets/4b4b835f-d931-40db-a099-6452f8f22246)

![KFE_on_robot_zoomed](https://github.com/user-attachments/assets/17dc9087-57af-4c59-bc60-92c8d33d5afe)

<iframe width="560" height="315" src="https://www.youtube.com/embed/64BjdOqDLk0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Note that not only does the filter nicely align with the sensor measurements, it is much more smooth than the distance extrapolator used previously highlighting the advantage of using the Kalman filter. 

 ![extrapolated_measured_distance](https://github.com/user-attachments/assets/1a06ada3-7bad-448a-8017-84f29d50fb18)

# Conclusion

Despite just being a few more lines of code, the Kalman filter is a power tool which allows a robot to really get the most out of its sensors. I am very glad I had the oppertunity to implement it on a real robotic system and witness the preformance benifits it brought.
 























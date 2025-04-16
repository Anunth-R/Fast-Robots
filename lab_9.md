---
layout: default
title: Lab 9
---

# Orientation Control
To map the environment, I selected using orientation control to spin my robot around and collect data at each point. This is because orientation control gives much more fine control of the robot's angle compared to open loop control. In addition, by stopping at every angle, I can use a longer ranging time compared to velocity control to gain more accurate distance measurements. The code I wrote to accomplish this is shown below and executed in the main loop.

![image](https://github.com/user-attachments/assets/86560c6c-b684-464c-93a0-bef680c42ffa)

# Code Breakdown

* The code begins by storing the initial orientation of the robot and setting that to the first target for the robot to reach.

![image](https://github.com/user-attachments/assets/a548f406-81e8-463a-9c39-25caf3288bf2)

* Then, if we need a new target, we compute a new target using the compute_next(target) helper function.

![image](https://github.com/user-attachments/assets/716e1a6a-8a18-4cd5-a77e-10fee7caf757)

* Once our target has been computed, we call our scan_controller(float reading, float desire) which moves the robot to a desired orientation.

![image](https://github.com/user-attachments/assets/e58a7c89-b608-4b66-adf5-75e5e9686b87)

* If the get_meas flag is triggered, we take the measurement.

![image](https://github.com/user-attachments/assets/9ca0a6aa-d0c7-4a69-8375-8e014b1b03b8)

Here is the implementation of the compute_next(target) helper function. It simply adds 20 degrees accounting for rap around. 

![image](https://github.com/user-attachments/assets/6a002e6c-ec99-443b-a8ac-d28bf3d8277a)

Finally, the scan_controller(float reading, float desire) simply wraps the previously implemented orientation controller but sets get_meas to true once the target has been reached.

# Polar Plots

During each scan, I collected 19 measurements at 20 deg increments (2 deg error bound) with a 100ms ranging time. After collecting raw data at each location, I created some polar plots to verify that the robot was outputing reasonable data. The plots are shown below. These plots seem to largely match my expectations with smooth lines near close boundries and spikes where there are no obstacles.

![image](https://github.com/user-attachments/assets/b0b7cbf3-a559-41ff-bb11-053b72035285)

Here is a video of my robot preforming a scan. As shown below, the robot does a fairly good job of turning in place without translating too much.

<iframe width="560" height="315" src="https://www.youtube.com/embed/mGzt2U54KEw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

To further verify the quality of the controller, here is a plot depicting the delta theta between the angle at which each TOF measurement was recorded. As shown below, each delta theta is within the 2 deg margin of error. Based on the above, the robot should produce a reasonably accurate map as the deviations in the robot's position is small compared to the 4m x 4m room the robot is placed in. In addition, the DMP produces very accurate robot yaw estimates only increasing the accuracy of the map. The limiting factor is likely the TOF measurements especially at long distances as this has proven to be unreliable in previous labs.

![image](https://github.com/user-attachments/assets/7f43080b-74d4-4298-830e-18642e9d7e5a)



# Transformations

With the raw data collected and looking reasonable, I next transformed the data into the coordinate frame of the map. I began by writing a function that transformed my measured angles so that they were consistent with the map frame. This included changing counter-clockwise rotation from negative to positive theta, mapping angles from [-180,180] to [0,2pi], and accounting for the starting orientation of the robot relative to the map. 

![image](https://github.com/user-attachments/assets/641ad044-aab2-48ab-9f3c-d751ab81f4a0)

Then, to transfrom the polar coordinates to rectangular coordinates in the frame of the map, I used the following homogeneous transform. 

![image](https://github.com/user-attachments/assets/95a197c5-b708-4d98-b74b-303f61b6bd33)

This matrix multiplies a vector of the following form.

![image](https://github.com/user-attachments/assets/58091ccc-2673-411a-96a6-df81cf2c33ae)

For my robot, the TOF sensor is located 2.3in from the front of my robot so I added that to all of the measurements.

# Map

After applying the following transfroms onto the collected data, i got the following map. The blue dotted line represents the approximate boundries of the environment from the collected data. As shown below, the sensor scans from each location show a reasonably accurate representation of the robot's environment. There are quite a few sources of error like not placing the robot in the exact same orientation at each location or erronious sensor measurements. However, despite this, the robot still produced a reasonably accurate map. 

![image](https://github.com/user-attachments/assets/6d863777-ca89-4b2d-9f85-9c99b8315184)

The end points of each of these line sigments will be stored so that it can be loaded into the simulator for future labs. 

# Discussion
I really enjoyed putting together many pieces from previous labs to map and environment. I look forward to using this map to localize my robot in future labs!.=



















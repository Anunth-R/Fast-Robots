---
layout: default
title: Lab 9
---

# Orientation Control
To map the environment, I selected using orientation control to spin my robot around and collect data at each point. This is because orientation control gives much more fine control of the robot's angle compared to open loop control. In addition, by stopping at every angle, I can use a longer ranging time compared to velocity control to gain more accurate distance measurements. The code I wrote to accomplish this is shown below and executed in the main loop.

![image](https://github.com/user-attachments/assets/86560c6c-b684-464c-93a0-bef680c42ffa)

# Code Breakdown

1. The code begins by storing the initial orientation of the robot and setting that to the first target for the robot to reach.

![image](https://github.com/user-attachments/assets/a548f406-81e8-463a-9c39-25caf3288bf2)

2. Then, if we need a new target, we compute a new target using the compute_next(target) helper function.

![image](https://github.com/user-attachments/assets/716e1a6a-8a18-4cd5-a77e-10fee7caf757)

3. Once our target has been computed, we call our spin controller which moves the robot to a desired orientation.

![image](https://github.com/user-attachments/assets/e58a7c89-b608-4b66-adf5-75e5e9686b87)

4. If the get_meas flag is triggered, we take the measurement.

![image](https://github.com/user-attachments/assets/9ca0a6aa-d0c7-4a69-8375-8e014b1b03b8)








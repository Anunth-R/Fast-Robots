---
layout: default
title: Lab 10
---

# Bayes Filter

In this lab, I implemented a Bayes Filter which will allow my robot to localize itself in its world environment descritized to a 12 x 9 x 18 grid. This filter requires a motion model and sensor model for the robot. Then using the control inputs supplied to the robot and recieived sensor measurements, the robot can use these models to probabilistically compute its most likely location in the grid. Algorithm is shown below.

![image](https://github.com/user-attachments/assets/2faed091-e5be-46e9-8732-343f27bd75fb)

# Implementation
In the provided skeleton code, implementing Bayes Filter requires implementing 5 functions. Each of these functions are discussed in the subsequent sections.

* compute_control
* odom_motion_model
* prediction_step
* sensor_model
* update_step
  



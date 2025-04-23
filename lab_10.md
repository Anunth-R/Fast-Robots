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

# Compute Control

This function takes in the previous and current state of the car and estimates the control inputs supplied to the car. It does this using the following kinematic model. 

![image](https://github.com/user-attachments/assets/8b6ff887-404d-4e09-a81c-fc9214ab948f)

The code to implement this is shown below.

![image](https://github.com/user-attachments/assets/ce51f268-afa4-48e5-ac98-299c4f30b85f)


  



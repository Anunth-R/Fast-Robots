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

# compute_control

compute_control in the previous and current state of the car and estimates the control inputs supplied to the car. It does this using the following coordinate relations. 

![image](https://github.com/user-attachments/assets/8b6ff887-404d-4e09-a81c-fc9214ab948f)

To implement this, we can leverage the equations shown above to compute the rotation and translation control inputs.

![image](https://github.com/user-attachments/assets/ce51f268-afa4-48e5-ac98-299c4f30b85f)

# odom_motion_model

odom_motion_model leverages compute_control to determine the control inputs supplied to the robot given two successive states. Then assuming Gaussian noise, the the probability of the given transition occuring with the supplied control input can be computed by sampling from three Gaussian distributions and multiplying them together.

![image](https://github.com/user-attachments/assets/e6c8a608-47d1-4efd-aa03-b78f6b4b9d19)

# prediction_step

With the above functions defined, we can effectively compute the probability of a robot reaching any state given an initial state and control inputs. Thus, the predict step essentially loops through each state in the map and computes the likelyhood that it reached that state given its prior state probabilities. Note that any probability smaller than 0.0001 is not pushed through the predict step to ease computation.

![image](https://github.com/user-attachments/assets/73c1e4e5-74d7-484a-8ba1-f376fd5ef47d)

# Sensor Model

The sensor model calculates the probability of recieving a sensor measurement given the robot's position. It does this by simply sampling from a gaussian distribution.

![image](https://github.com/user-attachments/assets/bb675ab8-895a-487b-8e6d-c7a657ce3e2f)


# Update Step 

Once the robot scans its environment with its ToF sensor, it can update its belief about its location using the sensor model defined previously. Note that this code was made more efficient my utiizing matrix multiplication and the precomputed obs_views data.

![image](https://github.com/user-attachments/assets/26f2a850-fc7e-459a-b752-f49b4d581451)

# Testing

Below are recorded experiments of the robot localizing itself using the above algorithm. 

## Trial 1

![table_2v2](https://github.com/user-attachments/assets/6c710891-febf-4d4c-9d6a-5585627295f3)

<iframe width="560" height="315" src="https://www.youtube.com/embed/mGzt2U54KEw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Trial 2

![table 3v2](https://github.com/user-attachments/assets/b77cb0e9-ce45-4825-92fa-ee95c9985d8b)

<iframe width="560" height="315" src="https://www.youtube.com/embed/oWmnsIwqayo" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

From the above videos,the robot does a good job overall of localizing the robot's position in the map. However, the filter might be a little bit overconfident of the robot's localization as a lot of the time the probability of the belief was 1 while in reality there was still some positional error. In addition, the localization seems to work best in narrow corridors or channels and worse in open spaces. This makes sense as the closer the obstacles, the more reliable the sensor measurements. The other thing to note is that the odometry appears to be completely off from the robot's actual position. Therefore, it is good that we are fusing these measurements with ToF measurements rather than relying on ToF measurements alone. 




















  



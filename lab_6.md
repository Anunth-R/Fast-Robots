---
layout: default
title: Lab 5
---

# Prelab

Most of the infrastructure for this lab had already been implemented in the previous lab. However, I added a flags start_IMU and start_O_controller to start sampling the IMU and running the orientation pid controller. I also created accociated BLE commands START_IMU, STOP_IMU, START_O_CONTROLLER, and STOP_O_CONTROLLER to command these flags. Finally, I slightly modified my command_motors function so that I could also supply a minimum PWM to account for deadband as an aguement. This is because moving straight and turning have different minimum PWM times.

![image](https://github.com/user-attachments/assets/44572e98-0bb9-4915-b9b0-b2a97e77cacc)

# DMP Setup

One thing that I noticed in the previous labs was that the yaw angle has substantial drift on the IMU. I found that I could correct this using the digital motion processor (DMP) built into the IMU but disabled by default. The DMP does this by fusing the accelerometer, gyroscope, and magnetometer data so that a single quaternion can be constructed that encodes the orientation of the robot. After reading through the documentaition and enabling the DMP, I added the following code to my setup to initialize DMP. 

![image](https://github.com/user-attachments/assets/8c299303-5281-443f-950d-76a64afa2f1b)

One important thing to note is that INV_ICM20948_SENSOR_GAME_ROTATION_VECTOR sets the maximum sampling rate of the IMU to 1.1kHz and the maximum detectable rotational speed to 2000 degrees per second. Both of these should be more than enough for the dynamics profile of this car. We can change the sample rate but the current one seems more than adaquate. 

The DMP outputs sensor measuments as a quaternion. Therefore, both polling the IMU and computing the yaw euler angle ([-180,180]) from the quaternion is handled by the IMU_DMP_YAW function shown below. Note that this code was heavily inspired by Example7_DMP_Quat6_EulerAngles.ino in the examples folder. 

![image](https://github.com/user-attachments/assets/fde2d86d-9aaf-498a-b44a-018b66625c61)

# Orientation PID Control

The code for my orientation pid controller is shown below and very  similar to the code used for the linear PID controller in the previous lab.
![image](https://github.com/user-attachments/assets/187f7e12-2b88-40d5-bd4b-3a696e4b1020)




---
layout: default
title: Lab 5
---

# Prelab

Most of the infrastructure for this lab had already been implemented in the previous lab. However, I added a flags start_IMU and start_O_controller to start sampling the IMU and running the orientation pid controller. I also created accociated BLE commands START_IMU, STOP_IMU, START_O_CONTROLLER, and STOP_O_CONTROLLER to command these flags. Finally, I slightly modified my command_motors function so that I could also supply a minimum PWM to account for deadband as an aguement. This is because moving straight and turning have different minimum PWM times.

![image](https://github.com/user-attachments/assets/44572e98-0bb9-4915-b9b0-b2a97e77cacc)

# DMP Setup

One thing that I noticed in the previous labs was that the yaw angle has substantial drift on the IMU. I found that I could correct this using the digital motion processor (DMP) built into the IMU but disabled by default. The DMP does this by fusing the accelerometer, gyroscope, and magnetometer data so that a single quaternion can be constructed that encodes the orientation of the robot. After reading through the documentation and enabling the DMP, I added the following code to my setup to initialize DMP. 

![image](https://github.com/user-attachments/assets/8c299303-5281-443f-950d-76a64afa2f1b)

One important thing to note is that INV_ICM20948_SENSOR_GAME_ROTATION_VECTOR sets the maximum sampling rate of the IMU to 1.1kHz and the maximum detectable rotational speed to 2000 degrees per second. Both of these should be more than enough for the dynamics profile of this car. We can change the sample rate but the current one seems more than adaquate. 

The DMP outputs sensor measuments as a quaternion. Therefore, both polling the IMU and computing the yaw euler angle ([-180,180]) from the quaternion is handled by the IMU_DMP_YAW function shown below. Note that this code was heavily inspired by Example7_DMP_Quat6_EulerAngles.ino in the examples folder. 

![image](https://github.com/user-attachments/assets/fde2d86d-9aaf-498a-b44a-018b66625c61)

# Orientation PID Control

The code for my orientation pid controller is shown below and very similar to the code used for the linear PID controller in the previous lab.

![image](https://github.com/user-attachments/assets/187f7e12-2b88-40d5-bd4b-3a696e4b1020)

Note that I added the following logic to prevent the transition from -180 to 180 degrees from causing spikes and instability in my controller.

![image](https://github.com/user-attachments/assets/176fbf3b-3ccc-4765-ba68-d43f307b85b4)

One thing to note is that similar to the previous lab, I took the derivative of the sensor measurement rather than the error itself. This prevents derivative kick in my controller. In addition, although the IMU outputs raw rotation rates, the DMP only gives angle measurements so I was forced to descretely differentiate those measurements. To ensure that the transition from -180 degrees to 180 degrees does not cause a massive spike in velocity, I added similar logic to that used on the error term.

![image](https://github.com/user-attachments/assets/751f2a73-e214-4fc6-a41c-dc9e0e558416)

Finally, just like the linear controller, I have a low pass filter to remove noise from my derivative term. 

![image](https://github.com/user-attachments/assets/dfab47d6-fb8a-4878-9bc9-ff95a85aed51)

With IMU_DMP_YAW and pid_O_controller implemented, I integrated these functions into my main control loop as shown below.

![image](https://github.com/user-attachments/assets/c0aadf2f-11bf-42e6-bb35-62eeadc37201)

# Testing the Controller

After some experimentation, I found that kp = 0.03, ki = 0.006, and kd = 0.08 had nice preformance. Below are plots and video of commanding my car to move from around -90 degrees to 30 degrees. As shown by the plots and video, the car can quickly change orientation to a new setpoint with an error threshold of less that 0.3 degrees.

![orientation](https://github.com/user-attachments/assets/2328423d-595f-4174-9cff-9a232ebf9661)

![control_sig](https://github.com/user-attachments/assets/fd840186-342b-41d1-89ed-c05fe49dc9dd)

# Improvements for the Future

I also added functionality so that I could dynamically change the setpoint of the orientation controller. The nature of my PID controller function made this easy to do as I simply had to add an extra command to allow this. In addition, my flag based implementation of my software makes it easy overall to enable/ disable different controllers and sensors which will hopefully make development easier in the future.

![image](https://github.com/user-attachments/assets/998b91c9-aa97-4f4c-871e-1d60c685b5aa)

# Windup Protection















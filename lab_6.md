---
layout: default
title: Lab 5
---

# Prelab

Most of the infrastructure for this lab had already been implemented in the previous lab. However, I added a flags start_IMU and start_O_controller to start sampling the IMU and running the orientation pid controller. I also created accociated BLE commands START_IMU, STOP_IMU, START_O_CONTROLLER, and STOP_O_CONTROLLER to command these flags. Finally, I slightly modified my command_motors function so that I could also supply a minimum PWM to account for deadband as an aguement. This is because moving straight and turning have different minimum PWM times.

![image](https://github.com/user-attachments/assets/44572e98-0bb9-4915-b9b0-b2a97e77cacc)

# DMP Setup


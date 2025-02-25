---
layout: default
title: Lab 3
---

# Prelab
I began by figuring out what the overall circuit for my sensors would look like. I had two long and two short I2C cables. Since the TOF sensors would be outside of the car, decided to attach the long I2C cables to my TOF sensors. These two I2C cables would then connect to a Qwiic MultiPort that would feature a third connection to the Artemis using a short I2C cable. The second short I2C would connect the IMU to the Qwiic MultiPort allowing all of the sensors to be wired. The wiring diagram is shown delow.

![Wiring Diagram (1)](https://github.com/user-attachments/assets/e07a25ec-351d-481c-b272-6307aef6f181)

One important thing to note is that according to the datasheet, both I2C sensors have an address of 0x52. Therefore, I needed to wire XSHUT for one of the sensors to the Artemis's A2 pin. This will allow me to disable that sensor and change its I2C address. In terms of sensor placement on the robot, I think that it will make sense to place one TOF at the front of the car and one TOF to the side to maximize coverage. 

# Lab Tasks

## Artemis Powered by a Battery
To allow my robot to move around freely, the Artemis will need to be powered independantly by a battery. To accomplish this, I removed the standard connector from a 750 mAh battery and soldered a JST connector onto it. This allows the Artemis to be directly powered by a battery as shown below.

![PXL_20250225_015845669 MP](https://github.com/user-attachments/assets/38cb4ff1-9f0c-4b6c-bb72-f7b5990165d1)

## Soldering TOFs and XSHUT
The next task was to solder the long I2C cables to the TOF sensors and the XSHUT pin of one TOF to the Artemis A2 pin. With this soldering complete, the circuit reflected in the wiring diagram can be assembled as shown below.

![PXL_20250225_020459719 MP](https://github.com/user-attachments/assets/154baf22-275b-4442-a4f8-6fcb6f057be9)

## Scanning for I2C
With the soldering complete, we can connect only a single sensor to the multiport and scan for its I2C address using the Wire_I2C example script. The output from the serial monitor is shown below.









---
layout: default
title: Lab 3
---

# Prelab
I began by figuring out what the overall circuit for my sensors would look like. I had two long and a short I2C cable. Since the TOF sensors would be outside of the car, decided to attach the long I2C cables to my TOF sensors. These two I2C cables would then connect to a Qwiic MultiPort that would feature a third connection to the Artemis using a short I2C cable. The wiring diagram is shown delow.

![IMU_Data](https://github.com/user-attachments/assets/b319c85b-8ae5-466d-8421-2baf38b8da8e)

One important thing to note is that according to the datasheet, both I2C sensors have an address of 0x52. Therefore, I needed to wire XSHUT for one of the sensors to the Artemis's A2 pin. This will allow me to disable that sensor and change its I2C address. 



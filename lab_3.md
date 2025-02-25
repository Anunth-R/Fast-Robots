---
layout: default
title: Lab 3
---

# Prelab
I began by figuring out what the overall circuit for my sensors would look like. I had two long and a short I2C cable. Since the TOF sensors would be outside of the car, decided to attach the long I2C cables to my TOF sensors. These two I2C cables would then connect to a Qwiic MultiPort that would feature a third connection to the Artemis using a short I2C cable. The wiring diagram is shown delow.

![Wiring Diagram (1)](https://github.com/user-attachments/assets/e07a25ec-351d-481c-b272-6307aef6f181)

One important thing to note is that according to the datasheet, both I2C sensors have an address of 0x52. Therefore, I needed to wire XSHUT for one of the sensors to the Artemis's A2 pin. This will allow me to disable that sensor and change its I2C address. 



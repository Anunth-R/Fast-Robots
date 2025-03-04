---
layout: default
title: Lab 4
---

# Prelab
To begin, I planned out my motor control circuit. From the previous lab, I had already used A2 to control the XSHUT pin on one of my TOF sensors. Therefore, I decided to use pins A0, A1 to control one motor driver and pins A4,A5 to control the other motor driver. The car's motor control circuit wiring diagram is depicted below.

![motor_wiring_diagram](https://github.com/user-attachments/assets/1e6dc659-1174-40bb-9515-ab70bd823dd6)

The motor drivers have been parrellel coupled to increase the amount of current they can handle. There are also two batteries in the circuit. The 850mah battery is used to power the motors while the 750mah battery powers the Artemis. This is because the current rewuired by the motors is much higher than the current required by the Artemis. Therefore, decoupling the power sources for the Artemis and motors prevent power surges from reseting the Artemis while reducing the noise of signals and allowing the car to last longer.

When building my circuit, I decided to try and make my wires as short as possible. I also tried to use stranded wire whenever I could to minmize the chance of a wire breaking. 

# Soldering The Motor Drivers


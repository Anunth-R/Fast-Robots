---
layout: default
title: Lab 4
---

# Prelab
To begin, I planned out my motor control circuit. From the previous lab, I had already used A2 to control the XSHUT pin on one of my TOF sensors. Therefore, I decided to use pins A0, A1 to control one motor driver and pins A4,A5 to control the other motor driver. The car's motor control circuit wiring diagram is depicted below.

![motor_wiring_diagram](https://github.com/user-attachments/assets/1e6dc659-1174-40bb-9515-ab70bd823dd6)

The motor drivers have been parrellel coupled to increase the amount of current they can handle. There are also two batteries in the circuit. The 850mah battery is used to power the motors while the 750mah battery powers the Artemis. This is because the current required by the motors is much higher than the current required by the Artemis. Therefore, decoupling the power sources for the Artemis and motors prevent power surges from reseting the Artemis while reducing the noise of signals and allowing the car to last longer.

When building my circuit, I decided to try and make my wires as short as possible. I also tried to use stranded wire whenever I could to minmize the chance of a wire breaking. 

# Soldering Motor Drivers to the Artemis/PWM Testing 
I began by parrellel coupling the motor drivers and soldering them to the Artemis. However, before soldering the motor drivers to the motors and 850 battery wires, I inspected the PWM signals outputed by the motor drivers. To accomplish this, I wrote Artduino code shown below that cycles between two PWM signals.

![image](https://github.com/user-attachments/assets/6ad7a34b-11e2-460d-9c29-e15524bce9e0)

I then hooked the outputs of a motor driver to an oscilloscope and powered them off of a DC power supply. I set the power supply to 3.7V with a 2A current limit as these settings would adaquately replicate the battery. While the motor drivers can theoretically handle more current, 2A should be sufficient for testing.

![osc_setup](https://github.com/user-attachments/assets/706c60a5-a5a0-4560-95d2-65f44f8f668b)

As shown in the video below, you can see the oscilloscope cycling between both PWM readings indicating that the motor driver is functioning properly. I then hooked up the other motor driver to verify that it was also functionng properly.

<iframe width="560" height="315" src="https://www.youtube.com/embed/PLM8cwRBdes" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>










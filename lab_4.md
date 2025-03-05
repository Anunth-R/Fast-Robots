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
I began by parrellel coupling the motor drivers and soldering them to the Artemis A0,A1,A4,A5 pins. However, before soldering the motor drivers to the motors and 850mah battery wires, I inspected the PWM signals outputed by the motor drivers. To accomplish this, I wrote Artduino code shown below that cycles between two PWM signals.

![image](https://github.com/user-attachments/assets/6ad7a34b-11e2-460d-9c29-e15524bce9e0)

I then hooked the outputs of a motor driver to an oscilloscope and powered them off of a DC power supply. I set the power supply to 3.7V with a 2A current limit as these settings would adaquately replicate the battery. While the motor drivers can theoretically handle more current, 2A should be sufficient for testing.

![osc_setup](https://github.com/user-attachments/assets/706c60a5-a5a0-4560-95d2-65f44f8f668b)

As shown in the video below, you can see the oscilloscope cycling between both PWM readings indicating that the motor driver is functioning properly. I then hooked up the other motor driver to verify that it was also functionng properly.

<iframe width="560" height="315" src="https://www.youtube.com/embed/PLM8cwRBdes" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# Dismatling the Car
To proceed further, I needed to take apart my car. I unscrewed the lid of the car and cut out the factory PCB. This will allow me to attach my own motor dirvers and control circuitry to the car.

# Soldering the Motors
With the factory PCB removed, I soldered the outputs of each of the motor drivers to the motors. I held off on soldering the motor drivers to the batteries and instead still powered them through the DC power supply.

![motor_power_supply](https://github.com/user-attachments/assets/e4c706cf-5aeb-4b89-99a0-020aee23666a)

I then wrote Arduino code to move one of the motors forwards and backwards. 

![image](https://github.com/user-attachments/assets/9f223d16-df4e-4704-a06b-95a32628ae28)

As shown below, the motors successfully move forwards and backwards using the motor drivers.

<iframe width="560" height="315" src="https://www.youtube.com/embed/V0_QVrS8n7w" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# Battery Power
Instead of powering the motor drivers off of an external power supply, I next powered them off of the 850mah battery. To do this, I soldered the battery power wires to the motor driver. I also switched to powering the Artemis off of its own 750mah battery. I then ran the same code above.

Below shows a video of one set of wheels spinning off of battery power. The car is now completely untethered from from any external sources.

<iframe width="560" height="315" src="https://www.youtube.com/embed/7-zsSJoXtN4" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

We can also command both motors to spin.

<iframe width="560" height="315" src="https://www.youtube.com/embed/zWqXxAawIWw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# Car Layout
With all of the soldering complete, we can now integrate all of these components into the car. Below shows an image of the current layout of my car. I decided to use rubber bands to secure the components as they seem to hold the components firmly in place while still allowing me to manipulate them when needed.

![Untitled drawing (2)](https://github.com/user-attachments/assets/7c6a0d0b-db09-474c-8def-7c2185360238)

# Minimum PWM
With my car fully integrated, I explored the minimum PWM I could supply to my motors that would still allow my motors to move forwards. I found that to somply move forwards, each motor required a PWM of atleast 35. Turning was a completely different story. I needed to supply a PWM of 100 to each motor in oposite directions to allow the car to turn. This however makes sense as the car has to overcome a lot more friction compared to just driving forwards. Therefore, a higher pwm signal is needed for each motor. 












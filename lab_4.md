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

# Dismantling the Car
To proceed further, I needed to take apart my car. I unscrewed the lid of the car and cut out the factory PCB. This will allow me to attach my own motor dirvers and control circuitry to the car.

# Soldering the Motors
With the factory PCB removed, I soldered the outputs of each of the motor drivers to the motors. I held off on soldering the motor drivers to the batteries and instead still powered them through the DC power supply.

![motor_power_supply](https://github.com/user-attachments/assets/e4c706cf-5aeb-4b89-99a0-020aee23666a)

I then wrote Arduino code to move one of the motors forwards and backwards. Here are definitions that will be used for the remainder of the code below.

![image](https://github.com/user-attachments/assets/2905c605-8470-4cee-8493-eec49239eaed)

This code that moves the motor in both directions.

![image](https://github.com/user-attachments/assets/9f223d16-df4e-4704-a06b-95a32628ae28)

As shown below, the motors successfully move forwards and backwards using the motor drivers.

<iframe width="560" height="315" src="https://www.youtube.com/embed/V0_QVrS8n7w" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# Battery Power
Instead of powering the motor drivers off of an external power supply, I next powered them off of the 850mah battery. To do this, I soldered the battery power wires to the motor driver. I also switched to powering the Artemis off of its own 750mah battery. I then ran the same code above.

Below shows a video of one set of wheels spinning off of battery power. The car is now completely untethered from from any external sources.

<iframe width="560" height="315" src="https://www.youtube.com/embed/7-zsSJoXtN4" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

We can also command both motors to spin. Code to do that is shown below.

![image](https://github.com/user-attachments/assets/b990fd34-ef9c-4c1f-ac51-de8da11728d6)

Here is a video.

<iframe width="560" height="315" src="https://www.youtube.com/embed/zWqXxAawIWw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# Car Layout
With all of the soldering complete, we can now integrate all of these components into the car. Below shows an image of the current layout of my car. I decided to use rubber bands to secure the components as they seem to hold the components firmly in place while still allowing me to manipulate them when needed.

![Untitled drawing (2)](https://github.com/user-attachments/assets/7c6a0d0b-db09-474c-8def-7c2185360238)

# Minimum PWM
With my car fully integrated, I explored the minimum PWM I could supply to my motors that would still allow my motors to move forwards. I found that to somply move forwards, each motor required a PWM of atleast 35. Turning was a completely different story. I needed to supply a PWM of 100 to each motor in oposite directions to allow the car to turn. This however makes sense as the car has to overcome a lot more friction compared to just driving forwards. Therefore, a higher pwm signal is needed for each motor. 

# Driving Straight
It is now time to drive the car! First logical task is to get it to go in a straight line. I began by simply applying a PWM signal of 80 to both motors. However, I found that my car veered to the left. Therefore, to make my car go straight, I found that I needed to multiply the PWM sent to my left motor by about 1.31. The code used to make the car drive straight is shown below.

![image](https://github.com/user-attachments/assets/5ef738f5-b869-44b4-a3de-3a0e1dc197a2)

Here is a video of the car driving straight for about 9ft.

<iframe width="560" height="315" src="https://www.youtube.com/embed/mbUZHdDoPtI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Open Loop Control
Now that the car can move straight, I tried to mess around with what I could do in open loop by making the car move in a box. Here is the code that I used to achieve that.

![image](https://github.com/user-attachments/assets/bc7bb1c7-cea0-450c-916d-0b6859bf1766)

Here is a video of it attempting the box. As you can see, it is not perfect. However, I think that PID control using the car's IMU will make its movement much more precise.

<iframe width="560" height="315" src="https://www.youtube.com/embed/mU4ZCq14TmE" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Analog Write Discussion
To determine the frequency of the pwm signal sent by analog read, we can look at the oscilliscope reading from the motor PWM test section. 

![image](https://github.com/user-attachments/assets/95b7b2f5-a309-427f-8818-b917b9463210)

As shown above in the lower right corner, the PWM frequency is about 183hz. One benifit of manually increasing this frequency is to deliver a smoother signal to the motor. The faster you can deliver a pwm frequency, the smoother the average voltage the motor will recieve. This will lead to less choppiness in your motor and overall better control of it. 


## Lowest PWM Value 
To explore the lowest PWM value once in motion, I provided a burst of pwm at 40 for half a second and experimented with how low I could lower the pwm for the remaining 4 seconds. I needed to use 40 rather than the 35 I had determined previously as my battery was less charged than before. I found that I could lower the right motor's pwm down to 34 and the left pwm down to 32 and still move the car forwards after the initial burst of pwm. Here is the code used to accomplish that. Based on the video, it seems that the car settles on its slowest speed really fast.

![image](https://github.com/user-attachments/assets/3a99add5-01a2-4c92-a8d6-ee16225555bc)

Shown below is a video demonstrating the car moving forward for about 4 seconds. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/mbEqu0SZwQQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


























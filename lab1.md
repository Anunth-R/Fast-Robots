---
layout: default
title: Lab 1
---

# lab 1A

## Prelab
During the prelab, I setup my Arduino IDE and installed the nessesary libraries.
## Task 1: Blink
To test basic functionality of the board, I ran the Blink.io file.

[task_1](https://youtube.com/shorts/4fWxn6_mYdg?feature=share)

## Task 2: Serial
After that, I tested my ability to send and recieve data over serial communication. This will be useful in the future when debugging my code. The messages were sent with a baud rate of 11520. 

[task_2](https://youtu.be/Ij7tu1WB8s4)

## Task 3: Analog Read/Temperature Sensing
The Artimis Nano has an onboard temperature sensor that can measure ambient temperature. By calling AnalogRead() in the Arduino code, we can convert the analog data from the temperature sensor into a digital signal that can be processed by the microcontoller and displayed on the serial monitor. The ambient temperature is around 3300 units. When I squeeze the board with my hand heating it up, the temperature increases to 33700 units indicating that the sensor is working properly.

[task_3](https://youtu.be/af0yhHFqj68)

## Task 4: Microphone
We can also test the microphone on the Artimis Nano which takes in analog sound and converts it to a digital signal that can be analyzed by the microcontroller using pulse density modulation(PDM). As shown in the video, the reading of the loudest frequency spikes whenever i snap my finger indicating that the microphone is working properly.

[task_4](https://youtu.be/KBXVvA9q3zI)

## Task 5: Musical C Note
Finally, I wrote code that detects when a musical c note is played and turns on the board's LED. If not, the LED turns off. To do this, I modified the code from task 4 to turn on the LED when the loudest frequency is between 500 and 600 units. I experimentally determined that this was the frequency range of a C note. 

[task_5](https://youtu.be/IyVRt3Y_dtY)

## Discussion
In this lab, I learned the basics of working with Arduino and the Artimis Nano board. This lab also help verify that my board is functioning correctly so that i can proceed with running more complex tasks on the Artimis in future labs. 

# Lab 1B

## Tasks ECHO
To test the Artemis board's ability to recieve data from Python, modify it, and send it back to the computer, I wrote a simple ECHO command. This command allows the Artemis to take in a command from python and echo it back with the modifier "Robot says -> " in front of it.

On the Arduino side:

![image](https://github.com/user-attachments/assets/9be4e9fd-648a-4a68-9f02-1754455f963c)

On the Python side:

![image](https://github.com/user-attachments/assets/1e29265e-0615-4098-a909-a721112b096f)









---
layout: default
title: Lab 1
---

## Prelab
During the prelab, I setup my Arduino IDE and installed the nessesary libraries.
## Task 1: Blink
To test basic functionality of the board, I ran the Blink.io file.
## Task 2: Serial
After that, I tested my ability to send and recieve data over serial communication. This will be useful in the future when debugging my code. The messages were sent with a baud rate of 11520. 
## Task 3: Analog Read/Temperature Sensing
The Artimis Nano has an onboard temperature sensor that can measure ambient temperature. By calling AnalogRead() in the Arduino code, we can convert the analog data from the temperature sensor into a digital signal that can be processed by the microcontoller and displayed on the serial monitor. The ambient temperature is around 3300 units. When I squeeze the board with my hand heating it up, the temperature increases to 33700 units indicating that the sensor is working properly.
## Task 4: Microphone
Finally, we can test the microphone on the Artimis Nano which takes in analog sound and converts it to a digital signal that can be analyzed by the microcontroller using pulse density modulation(PDM). As shown in the video, the reading of the loudest frequency spikes whenever i snap my finger indicating that the microphone is working properly.
## Discussion
In this lab, I learned the basics of working with Arduino and the Artimis Nano board. This lab also help verify that my board is functioning correctly so that i can proceed with running more complex tasks on the Artimis in future labs. 

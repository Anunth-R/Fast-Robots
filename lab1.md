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

Arduino code logic:

![image](https://github.com/user-attachments/assets/8d18ea60-318f-484b-85e2-18170345e88a)


[task_5](https://youtu.be/IyVRt3Y_dtY)

## Discussion
In this lab, I learned the basics of working with Arduino and the Artimis Nano board. This lab also help verify that my board is functioning correctly so that i can proceed with running more complex tasks on the Artimis in future labs. 

# Lab 1B

## Prelab/ Computer Setup

I ran the following commands to set up my python virtual environment. 

![image](https://github.com/user-attachments/assets/65ed7125-faf8-4eed-a4dd-7ddc181a5632)

![image](https://github.com/user-attachments/assets/0c8d0372-cc46-40c8-acac-4d095f54e826)

I also downloaded the BLE codebase and burned it on the Artemis. To verify that the Artemis was working properly, I verified that the MAC address was printed to terminal.

![image](https://github.com/user-attachments/assets/69ff4391-7152-4cf0-b1c8-ef0bbd8e08f1)

I then updated the MAC address in the connections.yaml folder to allow my computer to connect to the Artemis over bluetooth. I also generated a UUID to prevent accedently connecting to another board and updated both the arduino_ble and connections.yaml folder as shown below. 

![image](https://github.com/user-attachments/assets/a53bf4c4-68fc-4059-af10-a82f9a728382)

![image](https://github.com/user-attachments/assets/aec71ea0-9e10-4081-9264-8804db378a9a)

The following setup and codebase allows my computer and the Artemis board to wirelessly communicate with each other. When a message is sent from my laptop, the board uses its UUID to identify that the message came from my laptop and process it. In particular, the ble_arduino file handles the actual bluetooth communication on the board. In addition, the base_ble and ble python files handle communication on the Python side. Estring and RobotCommand also provide different kinds of messages that can be sent over bluetooth. 


## Task ECHO
To test the Artemis board's ability to recieve data from Python, modify it, and send it back to the computer, I wrote a simple ECHO command. This command allows the Artemis to take in a command from python and echo it back with the modifier "Robot says -> " in front of it.

On the Arduino side:

![image](https://github.com/user-attachments/assets/9be4e9fd-648a-4a68-9f02-1754455f963c)

On the Python side:

![image](https://github.com/user-attachments/assets/1e29265e-0615-4098-a909-a721112b096f)

## SEND_THREE_FLOATS

The next task was to demonstrate that I could send numerical data from Python, extract those floats in Arduino, and then print them to the serial monitor. To do this, I leveraged the robot_cmd.get_next_value() function to read and extract the floats sent from Python and then printed each of those floats seperated by a comma. 

On the Arduino side:

![image](https://github.com/user-attachments/assets/3bed9118-015b-49eb-a77f-d68b335e0a88)

On the Python Side:

![image](https://github.com/user-attachments/assets/b2893fb4-85e5-4c49-83b5-6bee1550df28)

When the Python code is run, the following message is printed in the serial monitor.

![image](https://github.com/user-attachments/assets/7b9b31aa-c1d7-42ff-a932-9e54700d12f4)

## Task GET_TIME_MILLIS

After that, I used the Artimis board's timer to send a message with the timestamp the message was sent at. To accomplish this, I created another command called GET_TIME_MILLIS.  I then leveraged the millis() function in Arduino to record the current time and send a message containing the recorded time.

On the Arduino side:

![image](https://github.com/user-attachments/assets/d00c4f3a-0cec-4455-9553-9ee3cd4ff095)

On the Python side:

![image](https://github.com/user-attachments/assets/87db72e7-414d-4c33-8624-00a02119079c)

## Notification Handler

To allow Python to process data whenever a message from the Artemis board is sent, we can set up a notification handler. The notification handler shown below extracts the time and message number and prints it. This handler will become useful for the next task. 

On the Python side:

![image](https://github.com/user-attachments/assets/15cde700-6530-4ae9-904d-43c7a3a4879a)

## Continuously Sending Messages

An important piece of information about our bluetooth setup is the rate at which information can be sent to our computer from the Artemis. To do this, I wrote a DATA_LOOP command that loops for 5 seconds. In each interation of the loop, the Artemis board sends the current time and message number to Python. The notification handler defined previously picks up these messages and prints them. During 5 seconds, 138 messages were outputted indicating a transmition rate of around 28 messages/sec. 

On the Arduino side:

![image](https://github.com/user-attachments/assets/bd82f941-4039-485c-81ba-8c5ab4f4a7dd)

On the Python side (only part of the messages shown):

![image](https://github.com/user-attachments/assets/0e26726c-9b4e-41ba-a9b0-ae818a269fe8)

## Sending Messages with an Array

Instead of continuously sending messages, another method of data transmission is to populate an array with a bunch of messages and then loop through the array and transmit those messages. To do this, I defined a global array with 50 slots called time_stamps. I then wrote an Arduino command SEND_TIME_DATA that populated this array with timestamps using a for loop. After that, I wrote a second loop that traverses the array and sends the timestamp and corresponding message number to Python. All 50 messages were sent in around 1ms indicating a much faster data transfer rate when sending messages already stored in an array. 

On the Arduino side:

![image](https://github.com/user-attachments/assets/d0eb6a01-92fd-4cab-8ced-66b3d263a3cf)


On the python side (only part of the messages shown):

![image](https://github.com/user-attachments/assets/62893a61-d14f-4fdd-9395-57067125bc35)

## Sending Temperature and Time Data Simultaneously

Using two arrays, we can send both time and temperature data. To accomplish this, I constructed a second aray called temps that had 50 slots just like Time_stamps. Then in a new Arduino command called GET_TEMP_READINGS, I implemented a while loop that filled up both arrays with time and temperature data. After that, a for loop transmited data from both arrays to Python. On the Python side, I implemented a new notification handler that used the recieved data to populate a corresponding temps and time_stamps array stored on my laptop. I then wrote code that called GET_TIME_READINGS which requested temperature data from the Arduino and code that printed the data from the arrays. 

Arduino Code:

![image](https://github.com/user-attachments/assets/809d2b57-69a1-413e-8f6b-10129f8b79c3)

Python Code (notification handler):

![image](https://github.com/user-attachments/assets/78e58d7e-a1b5-49c6-997d-978a3a0a74c5)

Python Code (Request Data):

![image](https://github.com/user-attachments/assets/312b98ee-8a3e-4e62-bdbc-55a2741e648d)

Python Code (Print Data, only part of messages shown):

![image](https://github.com/user-attachments/assets/48f1d7d6-541f-4115-8204-a6708bffe152)

## Tradeoffs Between Methods 

Both methods have their advantages and disadvantages. For sending small packets of data at an infrequent rate, live sending data might be better as the data is sent as soon as it is processed on the microcontroller. It also might be better for debugging. This is bacause the data is in real time which might make troubleshooting a lot easier. Therefore in senarios where you need continuous real time updates from sensors or for debugging, this method should be used.

The second method can transfer data really fast. It sent 50 messages in less than a millisecond. However, this data is not in real time. There could be a considerable lag between the data sent and what is currently happening on the robot. Another disadvantage is that all of the data must be stored on the Artemis which has limited memory. Therefore, this method is ideal for senarios where you want to transmit collected sensor data in bulk quickly for later processing on a computer. 

This begs the question of how much data can actually be stored on an Artemis? Our Artemis board has 384kb of storage. The data being transmitted is stored in floats which each take up 4bytes. Therefore, 96,000 data points of either temperature or time data can be stored before the Artemis Nano runs out of memory. 































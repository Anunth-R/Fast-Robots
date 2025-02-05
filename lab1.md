---
layout: default
title: Lab 1
---

# Lab 1A

## Prelab
During the prelab, I setup my Arduino IDE and installed the nessesary libraries.
## Task 1: Blink

[task_1](https://youtube.com/shorts/4fWxn6_mYdg?feature=share)

<iframe width="560" height="315" src="https://www.youtube.com/embed/4fWxn6_mYdg" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


## Task 2: Serial

The messages were sent with a baud rate of 11520. 

[task_2](https://youtu.be/Ij7tu1WB8s4)

## Task 3: Analog Read/Temperature Sensing
 When I squeezed the board heating it up, the temperature sensor reading increased  from 33300 to 33700 units indicating the code works.

[task_3](https://youtu.be/af0yhHFqj68)

## Task 4: Microphone
We can also test the microphone on the Artimis Nano. As shown in the video, the loudest frequency reading spikes whenever i snap my finger.

[task_4](https://youtu.be/KBXVvA9q3zI)

## Task 5: Musical C Note
Finally, I wrote code that turns on the board's LED when a musical c note is played. If not, the LED turns off. 

Arduino code logic:

![image](https://github.com/user-attachments/assets/8d18ea60-318f-484b-85e2-18170345e88a)


[task_5](https://youtu.be/IyVRt3Y_dtY)

## Discussion
In this lab, I learned the basics of working with Arduino and the Artimis Nano board. This lab also help verify that my board is functioning correctly.

# Lab 1B

## Prelab/ Computer Setup

I ran the following commands to set up my python virtual environment. 

![image](https://github.com/user-attachments/assets/65ed7125-faf8-4eed-a4dd-7ddc181a5632)

![image](https://github.com/user-attachments/assets/0c8d0372-cc46-40c8-acac-4d095f54e826)

To verify that the Artemis was working properly, I verified that the MAC address was printed to terminal when running the ble_arduino script.

![image](https://github.com/user-attachments/assets/69ff4391-7152-4cf0-b1c8-ef0bbd8e08f1)

I then updated the MAC address in the connections.yaml folder. I also generated a UUID to prevent accedently connecting to another board and updated both the arduino_ble and connections.yaml. 

![image](https://github.com/user-attachments/assets/a53bf4c4-68fc-4059-af10-a82f9a728382)

![image](https://github.com/user-attachments/assets/aec71ea0-9e10-4081-9264-8804db378a9a)

The following setup and codebase allows my computer and the Artemis board to wirelessly communicate with each other. The UUID prevents me from accidently connecting to another board.

## Task ECHO
I wrote a simple ECHO command that allows the Artemis to echo recieved commands back with "Robot says -> " in front of it.

On the Arduino side:

![image](https://github.com/user-attachments/assets/9be4e9fd-648a-4a68-9f02-1754455f963c)

On the Python side:

![image](https://github.com/user-attachments/assets/1e29265e-0615-4098-a909-a721112b096f)

## SEND_THREE_FLOATS

The next task was to extract three floats sent from my laptop and print them to the serial monitor. 

On the Arduino side:

![image](https://github.com/user-attachments/assets/3bed9118-015b-49eb-a77f-d68b335e0a88)

On the Python Side:

![image](https://github.com/user-attachments/assets/b2893fb4-85e5-4c49-83b5-6bee1550df28)

When the Python code is run, the following message is printed in the serial monitor.

![image](https://github.com/user-attachments/assets/7b9b31aa-c1d7-42ff-a932-9e54700d12f4)

## Task GET_TIME_MILLIS

After that, I used the Artimis board's timer to send a message with the timestamp the message was sent at.  I leveraged the millis() function in Arduino to accomplish this.

On the Arduino side:

![image](https://github.com/user-attachments/assets/d00c4f3a-0cec-4455-9553-9ee3cd4ff095)

On the Python side:

![image](https://github.com/user-attachments/assets/87db72e7-414d-4c33-8624-00a02119079c)

## Notification Handler

To allow Python to process data whenever a message from the Artemis board is sent, we can set up a notification handler. The notification handler shown below extracts the time and message number and prints it.  

On the Python side:

![image](https://github.com/user-attachments/assets/15cde700-6530-4ae9-904d-43c7a3a4879a)

## Continuously Sending Messages

An important piece of information is ble's data transfer rate. To determine this, I wrote a while loop that continuously sends timestamp messages for 5 seconds. The notification handler picks up these messages and prints them. During 5 seconds, 138 messages were outputted (28 messages/sec). 

On the Arduino side:

![image](https://github.com/user-attachments/assets/bd82f941-4039-485c-81ba-8c5ab4f4a7dd)

On the Python side:

![image](https://github.com/user-attachments/assets/0e26726c-9b4e-41ba-a9b0-ae818a269fe8)

## Sending Messages with an Array

Another method of data transmission is to populate an array with a bunch of messages and then send them. To do this, I defined a global array with 50 slots called time_stamps. I then populated this array with timestamps and then sent them to Python. All 50 messages were sent in around 1ms indicating a much faster data transfer rate. 

On the Arduino side:

![image](https://github.com/user-attachments/assets/d0eb6a01-92fd-4cab-8ced-66b3d263a3cf)


On the python side:

![image](https://github.com/user-attachments/assets/62893a61-d14f-4fdd-9395-57067125bc35)

## Sending Temperature and Time Data Simultaneously

Using two arrays in Arduino, we can send both time and temperature data. I created a new command GET_TEMP_READINGS which populated and sent these arrays. On the Python side, I implemented a new notification handler that used the recieved data to populate a corresponding arrays stored on my laptop. I then wrote python code that requested and printed the data from the python arrays. 

Arduino Code:

![image](https://github.com/user-attachments/assets/809d2b57-69a1-413e-8f6b-10129f8b79c3)

Python Code (notification handler):

![image](https://github.com/user-attachments/assets/78e58d7e-a1b5-49c6-997d-978a3a0a74c5)

Python Code (Request Data):

![image](https://github.com/user-attachments/assets/312b98ee-8a3e-4e62-bdbc-55a2741e648d)

Python Code (Print Data):

![image](https://github.com/user-attachments/assets/48f1d7d6-541f-4115-8204-a6708bffe152)

## Tradeoffs Between Methods 

For sending small packets of data at an infrequent rate, live sending data might be better as the data is sent as soon as it is processed on the microcontroller. It also might be better for debugging. This is bacause the data is in real time which might make troubleshooting a lot easier. 

The second method can transfer data really fast. It sent 50 messages in less than a millisecond. However, this data is not in real time. Another disadvantage is that all of the data must be stored on the Artemis which has limited memory. Therefore, this method is ideal for senarios where you want to transmit collected sensor data in bulk quickly for later processing on a computer. 

This begs the question of how much data can actually be stored on an Artemis? Our Artemis board has 384kb of storage. The data being transmitted is stored in floats which each take up 4 bytes. Therefore, 96,000 data points of either temperature or time data can be stored before the Artemis Nano runs out of memory.

## Effective Data Rate and Overhead

To determine how message size affects transmission rate, I created an Arduino command SEND_MESSAGE that takes in a message size and constructs/transmits a character array of that size (1 element = 1byte). On the Python side, I loop through different message sizes and time how long it takes to recieve a reply. As shown from the plot, many short messages induce more overhead reducing the data transfer rate. Sending longer messages reduces total overhead as less messages are sent. For example, the data transmission rate for 5 byte messages was 20.86 bytes/sec while for 120 byte messages was 501.81 bytes/sec. 

Arduino side:

![image](https://github.com/user-attachments/assets/8f837a90-5aa9-4beb-9011-6b9b8515a3f8)


Python side:

![image](https://github.com/user-attachments/assets/b23f7761-4f21-4a2b-8483-8661c736b0d5)


![image](https://github.com/user-attachments/assets/0915eacb-2433-470b-af7a-62bd471821f9)



## Reliability

To test reliability, I wrote an Arduino command called RELIABILITY which transfered 26 messages (containing the message number) with a given delay. I then wrote Python code that prints all of the messages recieved from the Arduino. I experimented with 0,20,100, and 200 ms delays. No matter the delay, all 26 transmitted messages were recieved in Python.

Arduino Side:

![image](https://github.com/user-attachments/assets/a4f12e6e-010e-4f2b-bc55-8c42eee194f1)

Python side

![image](https://github.com/user-attachments/assets/2ca27107-dd6f-45c6-9abc-6a00bd445025)

![image](https://github.com/user-attachments/assets/c302726d-3f12-46ce-b0e8-51bef0ec93f7)



## Discussion

In this lab, i gained familarity with BLE and refreshed by knowledge of Arduino/Python programming.































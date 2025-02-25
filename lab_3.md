---
layout: default
title: Lab 3
---

# Prelab
I began by figuring out what the overall circuit for my sensors would look like. I had two long and two short I2C cables. Since the TOF sensors would be outside of the car, decided to attach the long I2C cables to my TOF sensors. These two I2C cables would then connect to a Qwiic MultiPort that would feature a third connection to the Artemis using a short I2C cable. The second short I2C would connect the IMU to the Qwiic MultiPort allowing all of the sensors to be wired. The wiring diagram is shown delow.

![Wiring Diagram (1)](https://github.com/user-attachments/assets/e07a25ec-351d-481c-b272-6307aef6f181)

One important thing to note is that according to the datasheet, both I2C sensors have an address of 0x52. Therefore, I needed to wire XSHUT for one of the sensors to the Artemis's A2 pin. This will allow me to disable that sensor and change its I2C address. In terms of sensor placement on the robot, I think that it will make sense to place one TOF at the front of the car and one TOF to the side to maximize coverage. 

# Lab Tasks

## Artemis Powered by a Battery
To allow my robot to move around freely, the Artemis will need to be powered independantly by a battery. To accomplish this, I removed the standard connector from a 750 mAh battery and soldered a JST connector onto it. This allows the Artemis to be directly powered by a battery as shown below.

![PXL_20250225_015845669 MP](https://github.com/user-attachments/assets/38cb4ff1-9f0c-4b6c-bb72-f7b5990165d1)

## Soldering TOFs and XSHUT
The next task was to solder the long I2C cables to the TOF sensors and the XSHUT pin of one TOF to the Artemis A2 pin. With this soldering complete, the circuit reflected in the wiring diagram can be assembled as shown below.

![PXL_20250225_020459719 MP](https://github.com/user-attachments/assets/154baf22-275b-4442-a4f8-6fcb6f057be9)

## Scanning for I2C
With the soldering complete, we can connect a single sensor and IMU to the multiport and scan for its I2C address using the Wire_I2C example script. The output from the serial monitor is shown below.

![image](https://github.com/user-attachments/assets/5fa78ee2-7a00-4e98-a882-ab626bd3a597)

0x69 corresponds to the IMU and 0x29 corresponds to the TOF sensor. This means that we must change one sensor to an address that is not 0x69 and we should be good to go. One interesting note is that 0x29 does not match 0x52 from the datasheet. This is because the last bit is omitted from the address which halves its equivelent decimal value changing the address to 0x29.

## TOF Sensor Mode Selection and Data Collection
The TOF sensor has a short and long mode. The long mode gives longer sensor range while sacrificing some robustness to ambient light. Therefore, I selected the short mode as I envision that the TOF sensor will primarily be used for obstacle avoidance. In this use case, the sensor only needs to operate at close range so this mode seems optimal. Shown below is data outputed to the serial monitor using that mode. 

![raw_data_short](https://github.com/user-attachments/assets/5da3ab80-60ae-4598-b3b3-9b278ac141f2)

## TOF Sensor Preformance
To evaluate the preformace of the TOF sensor, I preformed two experiments. In the first experiment, I collected 100 samples of TOF data at each distance ranging from 2in to 50in at 2in intervals. The experimental setup is shown below.

![raw_data_short](https://github.com/user-attachments/assets/6545f95c-9fc0-4da5-b131-d07250d5771d)

Using the data from this experiment, I plotted the the measured vs true range of the TOF sensor as shown below.

![dis_vs_dis](https://github.com/user-attachments/assets/799bd926-070a-49d3-8b15-e14c0a554cd7)

From the data above we can see that the the sensor is fairly accurate in the intermediate range of the data. However, the closest data point seems to deviate a littel bit from the true value indicating that the sensor likely is not the best at extremely close range (within 2in). This is important to keep in mind when preforming obstacle avoidance with the robot.

We can further analyze this data by plotting the standard deviation of the data at each distance as shown below.

![std](https://github.com/user-attachments/assets/75a952f9-dd5b-4ef5-9bc4-491badc1122a)

The standard deviation generally increases as the distace increases which makes sense. However, it is still within around 2mm even at the furthest distance. Therefore, it appears that the sensor is very precise in its measurements and has little random noise.

Finally, we can plot the error of the sensor as a function of distance. 

![error_dis](https://github.com/user-attachments/assets/2282052a-3606-4bd7-b2cb-e33e2c3c865a)

As shown from the data above, the sensor is not perfectly accurate but some of this inaccuracy may come from my experimental setup. I was simply pushing a sensor attached to a box along a measuring tape which may not lead to the most reliable experimental data. However, what is evident is that the sensor is not accurate at close range. It deviated by over 20mm (0.78in) when measuring at a distance of 50.8mm (2in) which is horrible preformance. Therefore, it is important ot reiterate that the sensor should not be relied on at close range. However, it will produce good data at distances beyond that. Based on my experimentation, I did not find a max range of the data as the sensor still produced good data at 50in (1270mm). But the datasheet says the max range is 1.3m so it would be good to stay within that.

## 2 TOF Sensors

To use two TOF sensors at the same time, I wrote the following code to disable one sensor using XSHUT and change its I2C address. I changed the TOF sensor's address to 0x54. 

![image](https://github.com/user-attachments/assets/6fbe0f71-81fe-4f76-9937-a81f5f521344)

Then, using the code shown below I collected data from both TOF sensors.

![image](https://github.com/user-attachments/assets/98108f13-34ce-40e0-a7fd-68cb6cadd584)

Here is a video demonstrating this.

## TOF Sensor Sampling Speed

To test the sampling speed of the TOF sensors, I wrote code that prints the time in milliseconds every interation of the loop and the data from the 2 TOF sensors if available. 

![image](https://github.com/user-attachments/assets/b22391f2-3c4f-4a8f-8998-c62d353029a5)

This outputs the following.

![image](https://github.com/user-attachments/assets/a9e5a30c-fb4a-456a-bcd0-05ee0c00fd4d)

It takes about 10ms between loop interations and about 100ms between sensor measurements. Therefore, it is clear that the limiting factor is the TOF sensors as they can only be sampled when data is ready. To get a better idea of the ranging time for a sensor, I stored 100 measurements of TOF data with accociated time stamps in an array and transmitted the data over bluetooth. Below is a plot of the data.

![tof_fast](https://github.com/user-attachments/assets/fb1cb41f-adfd-4acc-a006-aaed20884f73)

It took about 5000ms to transmit 100 measurements so the ranging time of the sensor is about 50ms. The ranging time is not likely to change vs distance as the speed of light is much faster than the distances we are measuring. This leads to a max sampling rate of about 20Hz which is less than the datasheet of 50hz. However, this discrepency might be because of limitations of the Artemis board or the way the sensor library was implemented.


## 2 TOF Sensors and IMU over bluetooth

It is also important to be able to collect data from all sensors and transmit that data over bluetooth. To do this, I created functions for each sensor and called them in the main loop whenever data was avialable.

![image](https://github.com/user-attachments/assets/ed02e96c-2554-456e-8bd7-ece5da106aa7)

I also created bluetooth commands to start/stop recording data from all three sensors.

![image](https://github.com/user-attachments/assets/3ce39714-f39d-4e08-9b0c-f6c336fcd41f)

![image](https://github.com/user-attachments/assets/e9048d15-2f01-426c-8dda-b8a6a4c3a44f)





























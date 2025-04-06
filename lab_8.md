---
layout: default
title: Lab 8
---

# Prelab
In prior labs, my batteries had been placed in the battery compartment of the car without anything securing them other than the wires that connected them. However, in this lab, the car will be preforming high speed stunts which means that every component must be properly secured. Therefore, I added an extra rubber band to ensure that the batteries would not fall out of the car incase it crashed into a wall or flipped over.

![PXL_20250406_185308668 MP](https://github.com/user-attachments/assets/d1f4abfb-dc8a-4eb4-911d-d0ffbc816701)

The car is now ready to preform stunts!

# Drifting Steps
The drift stunt consists of three different steps.

* Approach: The car must quickly approach to 3ft from the wall.
* Drift: A quick 180 degree turn to orient the car towards its start direction.
* Return: The car returns as quickly as possible to the start line.

Therefore, I created a state variable called mode which will allow the car to keep track of which step it is currently on in the stunt. 

![image](https://github.com/user-attachments/assets/69ed1020-05b2-4c59-8350-382daab48243)

* mode = 0: Approach
* mode = 1: Drift
* mode = 2: Return

# Drift Controller
I then created a function called drift controller which handles each portion of the stunt.

![image](https://github.com/user-attachments/assets/05239efc-0ff3-4eb0-878f-e4ef15663f04)

## Approach
The approach step is handled by the following code snippet which essentially drives the car forward until the car is within 1214mm of the wall. Note that this is larger that the actual 914mm in the requirements because my TOF sensor is angled slightly upward and would therefore measure a longer distance than the actual distance to the wall.

![image](https://github.com/user-attachments/assets/4be40fe2-bc65-4eac-8771-b88b3d0fef1b)

## Drift
Then, once the car is close enough to the wall, we can initiate a 180 degree turn using the orientation controller implemented previously. Note, I added return statements to the orientation controller so that it would return 1 once the error is within the threshold and zero if it is not.

![image](https://github.com/user-attachments/assets/8353c3ab-c32b-4d00-984b-f10405e2b5fc)

I also created a helper function compute_oposite which computes the reference angle 180 degrees from the current orientation.

![image](https://github.com/user-attachments/assets/2fcbceae-80c4-43dc-8a50-fe97227b90aa)

## Return
The final part of the controller drives the car forward at full power for o.5s. This allows the car to return to the start line.

![image](https://github.com/user-attachments/assets/9cebac4c-399f-427e-bf0f-d2cf41f4d61d)

## Main Loop
Here is the controller implemented in the main loop of the code very similar to how the previous controllers were implemented.

![image](https://github.com/user-attachments/assets/76b7cb4a-07e4-4cac-bc8c-a48a3af327c4)

# Plots and Video
Here are plots and video of the the car drifting. As shown by the distance plot, the car approaches the wall and then spins around. As the car spins, the distance sensor becomes very noisy and large as it is now measuring an open room with no wall. Another interesting thing to note is that the orientation controller saturates a lot during the drift leading to uneeded oscillations. Therefore, it could have probably been tuned better to reduce these ocillations and the time it takes to complete the manuever.

![image](https://github.com/user-attachments/assets/8d0af167-9bb4-4d59-a754-3c0596e4b338)

![or_plot](https://github.com/user-attachments/assets/106cbd7a-7a0b-455e-b862-53b6b13083e2)

![u_plot](https://github.com/user-attachments/assets/cf894011-88d0-4d6b-bfe2-92fc0a04b3f1)

Here is the accociated video of the stunt.
























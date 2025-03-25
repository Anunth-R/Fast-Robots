---
layout: default
title: Lab 7
---

# Estimate Drag and Mass

In order to implement a Kalman filter, we need a linear state space model of our system. Our robot can be treated as a simple second order system with both a linear drag and motor force acting on it. If we construct the robot's state out of its position and velocity, we can model our robot using the state space system shown below.

[insert state space model]

This model requires knowledge of the drag force (d) and car mass (m). We can determine these quantities by analyzing our car's response to a step control input. The plot below depicts the car's TOF sensor measurements to a control input of 0.4.





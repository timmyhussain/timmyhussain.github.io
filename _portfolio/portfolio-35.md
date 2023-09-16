---
title: "Autonomous Control of eVTOL RC Aircraft<i></i>"
excerpt: "
* Developed a control architecture for control and guidance of vertical takeoff and landing RC aircraft using <b>ROS and Python</b><br>
<img src='/images/evtol.jpg' width=300px>"
collection: portfolio
permalink: "/portfolio/autonomous-eVTOL/"
published: true
date: 2019-08-31
tags:
  # - Embedded Software
  # - C Programming
  # - Modelling
  # - Simulation
---

## Summary

I worked on an awesome team of 3 to convert a fixed-wing RC aircraft into one capable of vertical takeoff and landing! We designed and 3D printed a frame and conducted some rudimentary propeller sizing to determine appropriate rotor sizes and required servo torque. I specifically focused on developing the flight control architecture for  communicating and controlling the aircraft by interfacing between the Pixhawk PX4 flight controller and Raspberry Pi computing platform. A lot of debugging and testing of the ROS-Python code was done using software in the loop (SITL) and the Gazebo simulator environment. 

<!-- ![Our trustee experimental platform](/images/evtol.jpg "Trustee Platform") -->
<img src="/images/evtol.jpg"  width="600" height="300">

## Approach
Use of the Pixhawk flight controller allowed us to abstract away some lower levels of control of the aircraft and deal with more higher-level commands. However, determining what commands had to be sent and when was a more difficult endeavor. A lot of the triggers would involve real-time information obtained from sensors such as altitude, climb rate or roll/pitch/yaw angles which involved establishing data flow to those sensors and processing them onboard the Raspberry Pi. The overall architecture ultimately took the form of a finite state machine with forward and backward transitions between related states triggered by the external environment. This included transitioning from quadrotor takeoff to fixed wing flight missions or loitering and back for landing. 

<!-- ![](/images/FSM2.png "Mode Controller State Machine") -->
<img src="/images/FSM2.png"  width="600" height="300">

<!-- [![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/LU2owCu6Pb4/0.jpg)](https://www.youtube.com/watch?v=LU2owCu6Pb4&t=23s) -->
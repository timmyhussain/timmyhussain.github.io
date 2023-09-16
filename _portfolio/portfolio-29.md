---
title: "Autonomous Vehicle Control using Reinforcement Learning and Search-Based Policy"
excerpt: "
* Reinforcement Learning based policy trained within AirSim simulation using <b>Python, TensorFlow & OpenAI Gym frameworks.</b><br/>
<img src='/images/airsim-env.gif' width=300px>"
collection: portfolio
permalink: "/portfolio/autonomous-control/"
published: false
date: 2022-01-30
tags:
  # - Machine Learning / AI
  # - Reinforcement Learning
  # - Optimal Control
  # - Path Planning
  # - Search Methods
  # - Simulation
  # - Robotics
---
## Key Takeaway
Implemented various control policies to solve the kidnapped-robot problem using a combination or the Microsoft AirSim simulation environment, the OpenAI Gym training framework for reinforcement learning and custom lower-fidelity simulations. 


## Summary

A Deep Learning (DL) approach to the kidnapped robot problem: a robot with no knowledge of its initial position within a known space navigating to known locations. Set up a training pipeline utilizing the AirSim 3D simulator and the OpenAI Gym training framework and successfully trained various policy models including Deep Q-Networks and Actor-Critic over many iterations using the Tensorflow/Keras DL framework. Ultimately due to time constraints switched to a simpler Search-based policy with limited depth and lookahead information. 

## Method

### Perception 
One-shot perception model trained on LiDAR data. 

### Control

RL formulation 

### Reward Crafting



### Forward Search Formulation


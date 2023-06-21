---
title: "Environmental Context Classification"
excerpt: "Trained a natural language model for autonomous vehicle environmental context classification using <b>Python & TensorFlow/Keras</b>" #<br/><img src='/images/500x300.png'>"
collection: portfolio
permalink: "/portfolio/context-classification/"
published: true
tags:
  - Machine Learning / AI
  # - Optimal Control
  # - Optimization
---

## Summary

Developed a sensor-agnostic environment context classification model which leveraged natural language 
to determine whether an autonomous vehicle was in urban, sub-urban, rural or open-sky environments. 

## Motivation
The project was borne from a research effort involving simulation-based testing of autonomous vehicle policies.
However, even if a simulation environment has high fidelity, we cannot naively assume failures happen with the 
same frequency or impact as real-life. To better steer the simulation environment to generate salient and 
relevant failures in a way consistent with real life, we wanted our sensors to observe environment features 
in the same way they would in real life. For example, blurry camera lenses on a rainy day or more noisy LiDAR
data profiles around more reflective objects -- a data-driven conditional observation model. 

While this is important for testing the policy, I was also interested in how we can train more intelligent policies.
For example, policiies which aer more flexible and can adapt based on their surroundings. The issue of environment
characterization is an important one for autonomous vehicle polciies. If you know you are in an environment where
you expect degradation of a certain sensor, you should be better positioned to decrease the weight of that sensor's
reading in your fusion algorithms. Sensor fault detection and exclusion. 

A problem with current environment characterization works at the time was they were very sensor-dependent. For example, 
GNSS environment characterization works involved only GNSS characteristics such as C/N0 or #satellites. This work
aimed to be sensor agnostic by relying only on natural language which is actually quite attainable especially for 
works which deal in scene segmentation and object detection of which there are many. It ultimately came down to a 
simple hypothesis which was to take in a bag of words output from your favorite scene segmenter and object detector
and train a neural network to match those words and output a distribution over target environment classes. 
---
title: "Machine Learning for Local Robot Localization"
excerpt: "
* Estimating likely positions from simulated LiDAR readings using Neural Networks, written using <b>Python & PyTorch.</b> <br>
<img src='/images/low-fidel.gif'>"
collection: portfolio
permalink: "/portfolio/neural-localization/"
published: true
date: 2022-01-30
tags:
  # - Machine Learning / AI
  # - Localization
  # - Perception
---

## Key Takeaway
Developed a Multi-Input, Multi-Output neural-network architecture that takes as inputs LiDAR measurements and an environment occupancy grip map and outputs a belief of the robots position over that map.

## Summary

In this work, we design a neural network framework for global localization given a known map. Our method solves for a
belief over a given map with no prior knowledge of the robot’s position (also known as the kidnapped robot problem). Our learning-based framework allows for more robustness to unexpected errors and enhanced generalization ability, with minimal modeling required, in contrast to model-based techniques. We train the network with LiDAR measurements, but avoid learning unnecessarily complex representations, such as LiDAR to images or intensity representations. We compare our approach against a model-based baseline and find that our algorithm performs equally well with a standard model-based method, while being faster.

## Approach

We assume a known map of the environment in which the agent of the problem is located. The map consists of an occupancy grid $M$ of dimensions $d_x \times d_y$, where each cell in the grid is either empty, occupied by an obstacle or occupied by the agent as shown in the figure.

<p align = "center">
  <img src='/images/map.png' width="300px"> 
</p>

We consider the localization task as a multiclass classification problem, where each class is a cell in the belief map. The network inputs are the LiDAR data and the known map. Using this information, we would like to make a prediction as to which cell(s) the robot is most likely to be in.

### Model

The overall architecture of the proposed model consists of two main components which are the map feature extraction layers and the LiDAR processing layers. The outputs of these layers are then concatenated and propagated through more processing layers before being passed through a dropout layer for regularization. The softmax activation function is applied in the final layer of the network, resulting in a multi-class probability distribution over the map. 

<p align = "center">
  <img src='/images/architecture2.png' width="100%"> 
</p>

Considering the map as a single channel image is what drove the decision behind using convolutional layers as part of the map feature extraction component. 
The width of the input and output layers are required to match the size of the input data and training position data.

### Data Collection

Data for training and for the simulations were collected using the AirSim simulation environment. Data was collected by initializing a vehicle with an on-board laser scanner to a random unoccupied position in the map with a random orientation in the range $[-\pi, \pi]$. Once the vehicle spawned in that location, the position and orientation of the vehicle as well as a LiDAR pointcloud measurement was captured and saved. This was repeated $15,300$ times. The figures below show the distribution of locations around a $10\times 10$ map from which data was collected.

|![](/images/airsim-env.gif)| ![](/images/10x10.png)|

### Data Processing
A characteristic of most neural networks is the requirement of fixed and defined input and output data sizes and types. The use of LiDAR measurements conflicts with this requirement due to the environment-and-sensor-dependent number of points detected by the sensor at any given time-step. We developed a data representation approach to avoid this problem by using fixed size vectors of size $720$, which consists of the mean ranges of LiDAR points at fixed angle increments of $0.5^o$. This is a transformation of the following form:

$$\mathbf{O_l} = 
\begin{bmatrix}
x_1, y_1\\
\vdots\\
x_n, y_n
\end{bmatrix}
\longrightarrow
\mathbf{O_l'} = 
\begin{bmatrix}
r_{\theta_1}\\
\vdots\\
r_{\theta_n}
\end{bmatrix}$$

where $r_\theta$ is the mean range of points at orientation $\theta$ and $x,y$ represent the $x$ and $y$ position of the points relative to the LiDAR laser scanner. 

Angle orientations at which there are no points in the measurement, are represented by a value of $-1$. The map representation is that of an occupancy grid. This results in a well defined data structure of fixed size $\mathbb{R}^{(d_x,d_y)}$ with values constrained between $0$ and $1$. True position data is represented in a similar way and is similarly bounded in the range $[0,1]$, as a belief. However, the 2D representation is not preserved and the data structure is instead flattened to a 1D array of size $\mathbb{R}^{d_x\times d_y, 1}$.


### Model Training and Performance

The model is trained on a Mean Squared Error (MSE) loss function which is defined as follows: 

$$L(y_\text{true}, y_\text{pred}) = \sum_{i=1}^\text{output size} \left( y^i_\text{pred} - y^i_\text{true} \right) ^2. $$ 


<p align = "center">
  <img src='/images/combined-gif.gif'> 
</p>

Our approach is able to circumvent the LiDAR-Image approach and associated computation overhead. It learns useful features and patterns from the data, performing demonstrably well on the map environment it is trained in. When the snapshot model is combined with a belief propagation method as demonstrated above, the algorithm is both accurate and fast enough to work in real time.
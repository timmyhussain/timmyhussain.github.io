---
title: "Optimizing Drone Fleet Routes"
excerpt: "
* Formulation of drone deivery, retailer and consumer network as optimization problem, written and simulated using <b>Python</b><br>
<img src='/images/demo-day.jpg' width=300px>"
collection: portfolio
permalink: "/portfolio/optimal-drone-fleet/"
published: true
author: Timmy Hussain, Brent Avery
date: 2021-12-24
tags:
  # - Optimization
  # - Operations Research
  # - Simulation
  # - Path Planning
---

## Key Takeaway

Developed a custom optimization framework for optimal drone fleet routing between multiple points of origin and multple destinations subject to power, distance and capacity constraints. 

## Summary

The goal of this project is to make a drone-order assignment which minimizes the energy cost associated with the delivery. We optimize the drone chosen subject to several constraints and formulate the routing problem as two independent linear programs which are solved with commercially available optimization solvers. 

## Method

The goal of this project is to develop a framework that is able to route a drone fleet around a series of locations in the context of delivering items from source nodes (retailers) to destination nodes (customers). Choosing optimal decisions by running optimization programs can often be an expensive process. As such, we've chosen an approach that minimizes the number of times an optimal decision needs to be made to two: once when an order is placed by a customer and once when the drone has delivered the package at the customer's location.

### Constraints

We have natural constraints which arise in this problem statement. 

| Constraint       | Definition                             |
| --------         | ------------------------------------------------------------ |
| Storage | The drone fleet is of a fixed size. They will be housed across a number of drone storage spaces (DronePorts) which are able to store comfortably only a limited number of drones. This is enforced in the architecture by setting a capacity variable for each DronePort object. |
| Power | Each drone has a fixed amount of battery life characterized by a maximum power variable. Charge Rate: Each drone charges at some fixed rate while at a DronePort location. Discharge Rate: Each drone discharges at some fixed rate while in transit. |
| Capacity | We will also constrain one drone to carry one item. The carrying capacity constraint is both enforced and ignored by both the packages and the drone's carrying capacity of unit weight. |

### Variables
- $x \in \mathbf{R}^{n_\text{drones}}$ where x represents an indicator vector for which drone we will choose i.e
$$
\begin{cases} 
  x_i = 1 & \text{we choose drone i} \\
  x_j = 0 & \forall j \neq i 
\end{cases}
$$
- $c \in \mathbf{R}^{n_\text{drones}}$ where $c_i$ represents the cost for drone i to make the delivery <br>
 $c_i$ = discharge rate $\times$ total mission distance for drone i
- $p \in \mathbf{R}^{n_\text{drones}}$ where $p_i$ represents the current power level for each drone. 

### Initial Optimization Problem
One reasonable formulation is as follows: <br>
<p align="center">
$$
\begin{equation}
\begin{aligned}
\min_{x} \quad & c^T x\\
\textrm{s.t.} \quad & c^T x \leq p \\
\quad & \sum_i x_i = 1\\
\quad & S^T x < C^T x \Longleftrightarrow S^T x + 1 \leq C^T x\\
\quad & x \succeq 0
\end{aligned}
\end{equation}
$$
</p>
However, while this formulation enforces all the constraints we care about, it doesn't give us a way out if we can't find a feasible solution. This is an unacceptable outcome as we cannot have our drone simply hovering over the customer's home until a spot is available. Therefore we propose an alternate formulation. 

### Modified Optimization Problem
This modified formulation tries to address the concerns brought up above. The rationale behind this is enabling a decision-making framework that incorporates a switching mechanism; this is the ability of a sufficiently powered drone at a full-capacity DronePort to switch locations thereby making room if needed. In this event, a drone would not end up stranded at a customer's location but rather a decision can be reached which routes the drone to its closest DronePort even if it is currently at full capacity by dispatching a switching drone to make room. <br>
<p align="center">
$$
\begin{equation}
\begin{aligned}
\min_{x} \quad & c^T x + \mathbf{c_2}^T x - \lambda \times \text{sign}(p - c)^T x\\
\textrm{s.t.} \quad & S^T x + 1 \leq C^T x\\
\quad & \sum_i x_i = 1\\
\quad & x \succeq 0
\end{aligned}
\end{equation}
$$ 
</p>
<br>
Where $\mathbf{c_2}$ is a vector of switching costs or 0 if not at full capacity.

### Performance
For one experiment, we decided to run a simulation where it turned out that one of the drones did not have enough fuel to fulfill its mission. This would be a semi-common occurrence in an actual online marketplace scenario, so it would be interesting to see how our system handles it.  Next, let's place some orders. Our first order will be from the middle retailer to the middle customer at $t = 0$. The next order will be from the leftmost retailer to the middle customer at $t = 15$. Let's see what decisions our system made from the path plot and the power level plot:

| ![](/images/battery.png) | ![](/images/PathBetter2.png)|

We see that at $t = 0$, our system decides to send out Drone A (Top section in Power Levels plot). Drone A travels out from DronePort 1, then goes to the retailer, travels over to the customer and then finally returns to DronePort 1 where it begins charging. First delivery completed without an issue. Next, at $t = 15$, we receive our second order and our system decides to send out Drone F (Bottom section in Power Levels plot) from DronePort 2. It goes to the retailer and then the customer as expected, but it seems that it has insufficient charge to return to DronePort 2. Because of this, it makes the next best decision, which is to attempt to go to DronePort 1 to charge. DronePort 1 is at capacity at $t = 30$, when Drone F arrives; fortunately, Drone A is sent from DronePort 1 to switch locations to DronePort 2 to make room for Drone F. 

[Download paper here](http://tinyurl.com/aa222-proj)
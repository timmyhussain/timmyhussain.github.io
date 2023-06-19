---
title: "Optimal Control of Payload-Carrying Quadrotor"
excerpt: "Receding Horizon Control framework for non-linear moveable payload problem, written using <b>Python, JAX & CVXPy.</b>" #<br/><img src='/images/500x300.png'>"
collection: portfolio
permalink: "/portfolio/quadrotor-payload/"
published: true
tags:
  - Optimization
  # - Optimal Control
  # - Path Planning
---

Developed a Receding-Horizon Control (RHC) framework addressing the problem of a payload-carrying quadrotor tracking a known reference trajectory. However, the dynamics of a quadrotor carrying a moveable payload are non-linear and coupled. To deal with this, some simplifications were made as well as some linearizations using JAX. Once linearized, it was easier to formulate as a receding-horizon problem solveable using conventional optimization packages. 
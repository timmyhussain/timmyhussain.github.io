---
title: "Optimal Control of Payload-Carrying Quadrotor"
excerpt: "
* Receding Horizon Control framework for non-linear moveable payload problem, written using <b>Python, JAX & CVXPy.</b>
<br/><img src='/images/dronedynamics.png' width=300px>"
collection: portfolio
permalink: "/portfolio/quadrotor-payload/"
published: true
date: 2021-05-30
tags:
  # - Optimization
  # - Optimal Control
  # - Path Planning
---

<html>
  <head>
    <title>Optimal Control of Payload-Carrying Quadrotor</title>
  </head>
  <body>
    <h1>Key Takeaway</h1>

    <p>Developed a Receding-Horizon Control (RHC) framework addressing the problem of a payload-carrying quadrotor tracking a known reference trajectory. However, the dynamics of a quadrotor carrying a moveable payload are non-linear and coupled. To deal with this, some simplifications were made as well as some linearizations using JAX. Once linearized, it was easier to formulate as a receding-horizon problem solveable using conventional optimization packages. 
    </p>
    
    <h1>Paper</h1>
    <object data="/files/AA_203_Project-fin.pdf" type="application/pdf" width="100%" height="100px">
      <p>Unable to display PDF file. <a href="https://tinyurl.com/AA203-proj">Download</a> instead.</p>
    </object>
  </body>
</html>

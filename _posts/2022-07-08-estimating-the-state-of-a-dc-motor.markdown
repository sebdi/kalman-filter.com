---
layout: post
title:  "Estimating the state of a dc motor"
date:   2022-07-08 00:00:00 +0000
categories: kalman-filter matlab
---

This applied example will focus on estimating the state of a DC motor. 
When modeling DC motors, it is important to mention that of course nonlinear models are superior to linear ones.

The starting point for modeling a DC motor is the electronic equivalent circuit and the basic equations of the DC machine.
An electric motor is an energy converter that converts electrical energy into kinetic energy. 

The electronic part consists of a series connection of the resistor R and the coil L as well as the electromechanical energy converter which represents the DC motor. 
This is controlled by a voltage source.

The energy converter is described by the basic equations of the DC motor as follows.
Induced voltage and the current. Motor torque and angular velocity.

Furthermore, by the approach of the total moment of inertia.

In the following, the state space model is derived from the described relationships.

For the electronic part, the following equation can be obtained using the mesh equation.

\\[ U = L \dot{i} + R i - Ue\\]

Since L is a differentiating term, we solve for the same.

\\[ \frac{di}{dt} = \frac{1}{L} u - \frac{R}{L} i - \frac{K_e}{L} \omega \\]

For the mechanical part, the torque equilibrium is started at the mechanical shaft of the motor.

\\[ J \dot{\omega} = m_m - m_w - m_R \\]

The two upper equations can now be transferred into the state-space description.
For this purpose, the following states are defined \\( \mathbf{x}_1 = \theta \\), \\( \mathbf{x}_2 = \dot{\theta} \\) and \\( \mathbf{x}_3 = i \\).

\\[ \mathbf{A} = \begin{bmatrix} 0 & 1 & 0 \\\\ 0 & \frac{-b}{J} & \frac{K_T}{J} \\\\ 0 & \frac{K_e}{L} & \frac{-R}{L}\end{bmatrix} \\]

\\[ \mathbf{B} = \begin{bmatrix} 0 \\\\ 0 \\\\ \frac{-R}{L}\end{bmatrix} \\]

\\[ \mathbf{G} = \begin{bmatrix} 0 \\\\ \frac{-1}{J} \\\\ 0\end{bmatrix} \\]

<h2>Discrete-time system description</h2>
Discretization of the matrices with the variables does not yield handy formulations.
Therefore, the discretization is done with concrete values for the variables. 
One obtains

\\[ \mathbf{A}_d = \begin{bmatrix} 0 & 1 & 0 \\\\ 0 & \frac{-b}{J} & \frac{K_T}{J} \\\\ 0 & \frac{K_e}{L} & \frac{-R}{L}\end{bmatrix} \\]

\\[ \mathbf{B}_d = \begin{bmatrix} 0 \\\\ 0 \\\\ \frac{-R}{L}\end{bmatrix} \\]

\\[ \mathbf{G}_d = \begin{bmatrix} 0 \\\\ \frac{-1}{J} \\\\ 0\end{bmatrix} \\]

<h2>Implementation and results</h2>

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

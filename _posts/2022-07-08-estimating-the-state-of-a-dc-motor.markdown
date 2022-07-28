---
layout: post
title:  "Estimating the state of a dc motor"
date:   2022-07-08 00:00:00 +0000
categories: kalman-filter matlab
---

This following example will focus on estimating the angular position \\( \theta \\), angular velocity \\( \dot{\theta} \\) and armature current \\( i \\) of a DC motor. 
When modeling DC motors, it is important to mention that certainly nonlinear models are superior to linear ones.
However, for didactic purposes and for a lot of applications, a linear model is sufficient for the time being.

The modeling of a DC motor is structured in two parts.
The electrical part is modeled with the electrical equivalent circuit and the kinematics with the basic equations of a DC motor.

<h2>Electrical system description</h2>
The electrical part consists a resistor \\( R \\) and the coil \\( L \\) that are connected in series as well as the electromechanical energy converter \\( M \\)  which represents the DC motor. 
The circuit is operated by the voltage source \\( U \\) .
The following equation can be obtained using the mesh equation.

\\[ U = L \dot{i} + R i - U_e\\]

The DC motor induces the voltage \\( U_e \\) that can be regarded linear to the angular velocity \\( U_e = k_e \dot{\theta} \\). The factor \\( k_e \\) accounts for different motor types.

Since \\(  L \dot{i} \\)  is a differentiating term, we solve for the same and get the following first order differential equation

\\[ \dot{i} = \frac{1}{L} U - \frac{R}{L} i - \frac{k_e}{L} \dot{\theta} \\]

that we can later transer to the state-space.

<h2>Kinematic system description</h2>
For the mechanical part, the energy converter is described by the approach of the total moment of inertia.
The torque equilibrium is started at the mechanical shaft of the motor

\\[ J \ddot{\theta} = m_m - m_R - m_L \. \\]

For the motor torque there is a linear relationship to the current \\( m_m = k_T i \\) that is adjusted by the factor \\( k_T \\). 
The friction torque \\( m_R \\) proportional to the angular velocity \\( m_R = b \dot{\theta} \\) with a coefficient \\( b \\).
The torque \\( m_L \\) is the mechanical load torque that is \\( m_L = 0 \\), if the DC motor is operated without payload.

\\[ J \ddot{\theta} = k_T i - b \dot{\theta} - m_L\\]

Since \\( \ddot{\theta} \\) is the term with the highest differentation, we solve for the same and get the following second order differential equation

\\[ \ddot{\theta} = \frac{k_T i}{J} - \frac{b}{J} \dot{\theta} - \frac{m_L}{J}  \\]


<h2>Continous time system description</h2>
The two upper equations can now be transferred into the state-space description.
For this purpose, the following states are defined \\( \mathbf{x}_1 = \theta \\), \\( \mathbf{x}_2 = \dot{\theta} \\) and \\( \mathbf{x}_3 = i \\).
Furthermore, we model that the system is only controled by the voltage supply, i.e., \\(\mathbf{u}(t) =  U \\) and the load torque is a unknown disturbance to the system with \\(\mathbf{v}(t) =  m_L \\).
The continous state-space descriptions becomes

\\[ \dot{\mathbf{x}}(t) = \mathbf{A} \mathbf{x}(t) + \mathbf{B} \mathbf{u}(t) + \mathbf{G} \mathbf{v}(t) \\]

with the following matrices

\\[ \mathbf{A} = \begin{bmatrix} 0 & 1 & 0 \\\\ 0 & \frac{-b}{J} & \frac{K_T}{J} \\\\ 0 & \frac{K_e}{L} & \frac{-R}{L}\end{bmatrix} \\]

\\[ \mathbf{B} = \begin{bmatrix} 0 \\\\ 0 \\\\ \frac{-1}{L}\end{bmatrix} \\]

\\[ \mathbf{G} = \begin{bmatrix} 0 \\\\ \frac{-1}{J} \\\\ 0\end{bmatrix} \\]

For the measurement equation

\\[ \mathbf{y}(t) =  \mathbf{C} \mathbf{x}(t) + \mathbf{z}(t)\\]

let's assume we measure the angular velocity

\\[ \mathbf{C} = \begin{bmatrix} 0 & 1 & 0\end{bmatrix} \\]

<h2>Discrete-time system description</h2>
Discretization of the matrices with the variables does not yield handy formulations.
Therefore, the discretization is done with the following parameters \\( b=0, J=3, K_T=3, K_e=3, R=0.1, L=0.1 \\). 
One obtains

\\[ \mathbf{A}_d = \begin{bmatrix} 0 & 1 & 0 \\\\ 0 & \frac{-b}{J} & \frac{K_T}{J} \\\\ 0 & \frac{K_e}{L} & \frac{-R}{L}\end{bmatrix} \\]

\\[ \mathbf{B}_d = \begin{bmatrix} 0 \\\\ 0 \\\\ \frac{-R}{L}\end{bmatrix} \\]

\\[ \mathbf{G}_d = \begin{bmatrix} 0 \\\\ \frac{-1}{J} \\\\ 0\end{bmatrix} \\]

<h2>Implementation and results</h2>

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

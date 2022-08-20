---
layout: post
title:  "Estimating the state of a dc motor"
permalink: /estimating-the-state-of-a-dc-motor/
date:   2022-07-08 00:00:00 +0000
categories: applications
---

This following example will focus on estimating the angular position \\( \theta \\), angular velocity \\( \dot{\theta} \\) and armature current \\( i \\) of a DC motor with a [linear Kalman filter](/linear-kalman-filter/). 
When modeling DC motors, it is important to mention that certainly nonlinear models are superior to linear ones.
However, for didactic purposes and for a lot of applications, a linear model is sufficient for the time being.

<h2>Continuous-time system model</h2>
The modeling of a DC motor is structured into two parts.
The electrical part is modeled with the electrical equivalent circuit and the kinematics with the basic equations of a DC motor.

<h3>Electrical system model</h3>
The electrical part consists of a resistor \\( R \\) and the coil \\( L \\) which are connected in series, and the electromechanical energy converter \\( M \\)  which is the DC motor. 
The circuit is operated by the voltage source \\( U \\).
The following equation can be determined using the mesh equation

\\[ U = L \dot{i} + R i - U_e \.\\]

The DC motor induces the voltage \\( U_e \\) that can be regarded linear to the angular velocity \\( U_e = k_e \dot{\theta} \\). The factor \\( k_e \\) accounts for different motor types.
Since \\(  L \dot{i} \\)  is a differentiating term, we solve for the same and get the following first order differential equation

\\[ \dot{i} = \frac{1}{L} U - \frac{R}{L} i - \frac{k_e}{L} \dot{\theta} \\]

that we will later transfer to the state-space.

<h3>Kinematic system model</h3>
For the mechanical part, the energy converter is described by the approach of the total moment of inertia.
The torque equilibrium is started at the mechanical shaft of the motor

\\[ J \ddot{\theta} = m_m - m_R - m_L \. \\]

For the motor torque \\(m_m\\) there is a linear relationship to the current \\( m_m = k_T i \\) that is adjusted by the factor \\( k_T\\). 
The friction torque \\( m_R \\) is proportional to the angular velocity \\( m_R = b \dot{\theta} \\) multiplied by the coefficient \\( b\\).
The torque \\( m_L \\) is the mechanical load torque that is \\( m_L = 0 \\), if the DC motor is operated without payload, but we will assume later unknown load torque.
Since \\( \ddot{\theta} \\) is the term with the highest differentiation, we solve for the same and get the following second order differential equation

\\[ \ddot{\theta} = \frac{k_T i}{J} - \frac{b}{J} \dot{\theta} - \frac{m_L}{J} \. \\]

<h3>Combined system model</h3>
The two upper equations can now be transferred into one state-space model.
For this purpose, the following states are defined \\( \mathbf{x}_1 = \theta \\), \\( \mathbf{x}_2 = \dot{\theta} \\), \\( \mathbf{x}_3 = m_L \\)  and \\( \mathbf{x}_4 = i \\).
Furthermore, we model that the system is only controled by the voltage supply, i.e., \\(\mathbf{u}(t) =  U \\) and the derivative of the load torque \\( m_L \\) is a unknown disturbance to the system with \\( \dot{\mathbf{x}}_3 = \mathbf{v}(t) \\).
The continuous state-space model becomes

\\[ \dot{\mathbf{x}}(t) = \mathbf{A} \mathbf{x}(t) + \mathbf{B} \mathbf{u}(t) + \mathbf{G} \mathbf{v}(t) \\]

with the following matrices

\\[ \mathbf{A} = \begin{bmatrix} 0 & 1 & 0 & 0\\\\ 0 & -\frac{b}{J} & -\frac{1}{J} &\frac{k_T}{J} \\\\ 0 & 0 & 0 & 0 \\\\ 0 & -\frac{k_e}{L} & 0 & -\frac{R}{L}\end{bmatrix} \\]

\\[ \mathbf{B} = \begin{bmatrix} 0 \\\\ 0 \\\\ 0 \\\\ \frac{1}{L}\end{bmatrix} \\]

\\[ \mathbf{G} = \begin{bmatrix} 0 \\\\ 0 \\\\ 1 \\\\ 0\end{bmatrix} \. \\]

For the measurement equation

\\[ \mathbf{y}(t) =  \mathbf{C} \mathbf{x}(t) + \mathbf{z}(t)\\]

let's assume we measure the angular positions with an absolute encoder

\\[ \mathbf{C} = \begin{bmatrix} 1 & 0 & 0 & 0\end{bmatrix} \ . \\]

*Some notes, the choice of state variables is the part of modeling and it is possible for both another choice and another order.
When modeling the load torque \\( m_L \\), it was assumed that the load torque \\( m_L \\) is unknown and noisy.
Sine the load torque \\( m_L \\) is not mean-free, it is not possible to set \\(  \mathbf{v}(t)  = m_L\\).
One practice is therefore to assume the change (i.e. the derivative) to be mean-free and to set the variable as an additional state (here \\( \mathbf{x}_3 \\)).*

<h2>Discrete-time system model</h2>
Discretization of the matrices \\(\mathbf{A}_d\\) and \\(\mathbf{B}_d\\) does not yield handy formulations if analytical discretized.
Therefore, the discretization is done with the following values \\( J=0.01 \\), \\(b=0.0001\\), \\(k_T=0.2\\), \\(k_e=4\\), \\(R=2.5\\), \\(L=0.0002\\), \\(\sigma_L=0.1\\) and \\(T_s=0.1\\) that are later also used for the simulation by using the known equations

\\[ \mathbf{A}_d = e^{\mathbf{A} T_s} \\]

\\[ \mathbf{B}_d = \int^{T_s}\_{0}  e^{\mathbf{A} \tau} d \tau  \mathbf{B}  \. \\]

For the sake of completeness, the discrete system model is
\\[ \mathbf{x}(k) = \mathbf{A}_d(k-1) \mathbf{x}(k-1) + \mathbf{B}_d(k-1) \mathbf{u}(k-1) + \mathbf{v}(k-1) \\]

\\[ \mathbf{y}(k) = \mathbf{C}(k) \mathbf{x}(k) + \mathbf{n}(k) . \\]

It is worth taking a look at the 3rd state variable \\(x_3\\) with the load torque
\\[ m_L(k) = m_L(k-1) + v_{m_L}(k-1) \.\\] 
The line is to be understood in such a way that the change of the load torque from time \\( k-1 \\) to time \\( k \\) corresponds to gaussian noise, i.e. \\( m_L(k) - m_L(k-1)= v_{m_L}(k-1)\\).

<h3>Observability</h3>
In order to check whether the modeling of the measurement mapping allows to observe all states, one can check the rank of the observability matrix
\\[ \mathbf{O} = \text{rank} \begin{bmatrix} \mathbf{C} \\\\ \mathbf{C} \mathbf{A}_d  \\\\ \mathbf{C} (\mathbf{A}_d)^2  \\\\ \mathbf{C} (\mathbf{A}_d)^3 \end{bmatrix} = 4 \. \\]
In this case, the rank corresponds to the number of degrees of freedom and thus the system is fully observable with the chosen measurement mapping \\( \mathbf{C}\\).

<h3>Determination of the system and measurement noise</h3>
For the calculation of the covariance matrix of the discrete process noise the discretization with zero-order hold is chosen
\\[ \mathbf{Q}_d = \int^{T_s}\_{0}  e^{\mathbf{A} \tau} \mathbf{Q} e^{\mathbf{A}^T \tau} d \tau  \. \\]
*The calculation does not provide a handy result for \\( \mathbf{Q}_d \\) therefore it is not given here.*

The determination of the measurement noise is based on the assumption that the angular position of the dc motor is determined by an absolute encoder with a resolution of 12 bits. This results in 4096 discrete positions, where one position corresponds approximately to an interval length of 0.088 degrees. If the 3 sigma deviation is used as a measure of scatter, i.e. that in 99.73 % of all cases the measured value is within the 3 sigma interval, then this corresponds for the measurement noise in a variance of
\\[  \sigma^2_r = 0.000225^{\circ} \. \\]

<h2>Implementation and results</h2>
The following results were received by conducting a simulation in MATLAB.
To make the simulation more lively and realistic, the input voltage is set to 6 V and in the middle of the simulation to 12 V to mock a controller (see the figure below).

<p align="center">
<img src="/assets/images/dc_motor/u.png" title="input voltage"/>
</p>

Figure 2 shows the course of the estimate of \\(x_1\\), the measurements y, and the ground truth of \\(x_1\\). 
It can be clearly seen that the estimate is closer to the ground truth than, to the measurement \\(y\\).

<p align="center">
<img src="/assets/images/dc_motor/x1.png" title="x1"/>
</p>

The previous result was mainly qualitative, a widely used way to better evaluate the estimation error is to use a metric such as the [Root Mean Square Error (RMSE)](/root-mean-square-error/).
Figure 3 shows the RMSE for \\( x_1\\) and the measurement. 
It can be seen that the state estimation of the Kalman filter has a substantially lower RMSE than if the measured value were used.

<p align="center">
<img src="/assets/images/dc_motor/RMSE.png" title="RMSE"/>
</p>

As the last step, the correctness of the simulation can be confirmed by the [Normalized Estimation Error Squared (NEES)](/normalized-estimation-error-squared/). 
Since 1000 Monte Carlo runs were performed, a 95% confidence interval \\( [3.83;4.18] \\) can be determined. 
Figure 4 shows the result and the NEES stays within the confidence interval.
Moreover, the mean of the NEES is close to 4 which is the degree of freedom of the used state-space system.

<p align="center">
<img src="/assets/images/dc_motor/NEES.png" title="NEES"/>
</p>

<h2>Source Code</h2>
The author is working on an extended code base which also includes the system shown above. 
For now, the code can only be made available after completing the larger code base. 
Stay tuned!


[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

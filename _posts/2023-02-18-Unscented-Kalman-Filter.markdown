---
layout: post
title:  "Unscented Kalman Filter"
description: The Unscented Kalman Filter (UKF) is the most known representative of the sigma-point methods.
permalink: /unscented-kalman-filter/
date:   2023-02-18 00:00:00 +0000
categories: kalman-filter
---

For the state estimation of nonlinear systems, shortly after the original publication of Rudolf Kalman, a variant was published by Kalman himself which allows the application of the Kalman filter also for nonlinear systems. 
Here, the nonlinear system is linearized at the current estimated value. 
For this the equations must be derived analytically. 
This procedure was for many years the solution for the nonlinear systems which occur more frequently in practice. 
An essential disadvantage of this method is that the filter can diverge easily since the estimated value does not correspond to the actual state and thus is always linearized at the wrong place. 
In the case of unfavorable nonlinearities, the estimated value diverges and thus linearization is performed at an even more incorrect position. 
This leads after some steps fast to a complete divergence. 

The famous publication of Simon J. Julier and Jeffrey K. Uhlmann approached the problem of estimating nonlinear systems differently [1]. 
It is based on the idea not to linearize the involved functions, since they are known exactly. 
Rather, a procedure must be used to choose points that can be sent through the nonlinear functions and then reassemble the results so that they can be worked with. 
This method went down in history with the name Unscented Transformation and the filter belonging to it has become world-famous as Unscented Kalman Filter (UKF).

<h3>Unscented Transformation</h3>
It results in the following calculation rules for the sigma points.
For \\( i=0,1,...,n_x \\) with \\( \text{dim}( \mathbf{x} ) = n_x \\) the sigma points are

\\[ \mathcal{X}^{(0)}(k-1) = \mathbf{x}(k-1\|k-1) ,\\]

\\[ \mathcal{X}^{(i)}(k-1) = \mathbf{x}(k-1\|k-1) + \sqrt{\frac{n_x}{1 - W_0}} \bigl(\mathbf{P}^{\frac{1}{2}}(k-1\|k-1)\bigl)_i , \\]

and

\\[ \mathcal{X}^{(i+n_x)}(k-1) = \mathbf{x}(k-1\|k-1) - \sqrt{\frac{n_x}{1 - W_0}} \bigl(\mathbf{P}^{\frac{1}{2}}(k-1\|k-1)\bigl)_i \\]

where the parenthesis \\( \bigl( \cdot \bigl)_i \\) indicates that the i-column of the matrix is to be used. 
The corresponding weight is computed with

\\[ W_i = \frac{1-W_0}{2n_x} .\\] 

Subsequently, the sigma points \\( \mathcal{X} \\) are used to be sent through the (nonlinear) function \\( \mathbf{f} \\) and by means of the weights \\( W_i \\) to calculate the first central moment

\\[ \mathbf{x}(k\|k-1) = \sum^{2n_x}_{i=1} \mathbf{f}(\mathcal{X}^{(i)}(k-1)) W_i  .\\]

The predicted covariance matrix of the predicted state results insignificantly different with additive addition of the process noise

\\[ \mathbf{P}(k\|k-1) = \sum^{2n_x}_{i=1} \bigl( \mathbf{f}(\mathcal{X}^{(i)}(k-1))-\mathcal{x}(k\|k-1) \bigl) \bigl( \mathbf{f}(\mathcal{X}^{(i)}(k-1))-\mathcal{x}(k\|k-1) \bigl)^T W_i + \mathbf{Q}_d(k-1)  .\\]

For the correction step, another set of sigma points must be calculated from the predicted state \\( \mathbf{x}(k\|k-1) \\) and the predicted covariance \\( \mathbf{P}(k\|k-1) \\),

\\[ \mathcal{X}^{(0)}(k) = \mathbf{x}(k\|k-1) ,\\]

\\[ \mathcal{X}^{(i)}(k) = \mathbf{x}(k\|k-1) + \sqrt{\frac{n_x}{1-W_0}} \bigl(\mathbf{P}^{\frac{1}{2}}(k\|k-1)\bigl)_i \\]

and

\\[ \mathcal{X}^{(i+n_x)}(k) = \mathbf{x}(k\|k-1) - \sqrt{\frac{n_x}{1-W_0}} \bigl(\mathbf{P}^{\frac{1}{2}}(k\|k-1)\bigl)_i  .\\]

To perform the correction step, the measurement vector must be predicted

\\[ \mathbf{y}(k\|k-1) = \sum^{2n_x}_{i=1} \mathbf{h}(\mathcal{X}^{(i)}(k)) W_i  .\\]

With the help of the intermediate variables, cross covariance

\\[ \mathbf{P}\_{xy}(k\|k-1) = \sum^{2n_x}_{i=1} \bigl( \mathcal{X}^{(i)}(k) - \mathbf{x}(k\|k-1)  \bigl) \bigl( \mathbf{h}(\mathcal{X}^{(i)}(k)) - \mathbf{y}(k\|k-1)  \bigl)^T W_i  \\]

and innovation

\\[ \mathbf{S}(k\|k-1) = \sum^{2n_x}_{i=1} W_i \bigl( \mathbf{h}(\mathcal{X}^{(i)}(k))- \mathbf{y}(k\|k-1) \bigl) \bigl( \mathbf{h}(\mathcal{X}^{(i)}(k))- \mathbf{y}(k\|k-1) \bigl)^T + \mathbf{R}(k)  \\]

the state 

\\[ \mathbf{x}(k\|k) = \mathbf{x}(k\|k-1) + \mathbf{P}_{xy}(k\|k-1) \mathbf{S}(k\|k-1)^{-1} (\mathbf{y}(k) - \mathbf{y}(k\|k-1) ) \\]

and covariance 

\\[ \mathbf{P}(k) = \mathbf{P}(k\|k-1) - \mathbf{P}\_{xy}(k\|k-1)  \mathbf{S}(k\|k-1)^{-1} \mathbf{P}\_{xy}(k\|k-1)^T \\]

can be updated.

<h3>Conclusion</h3>
The Unscented Transformation is fascinating and is a fundamental contribution to the use of deterministic methods for state estimation of nonlinear systems. 
A major disadvantage of the UKF that there is a tuning parameter with W. 

<h3>References</h3>
[1] Julier, Simon J.; Uhlmann, Jeffrey K. (1997). "New extension of the Kalman filter to nonlinear systems" (PDF). In Kadar, Ivan (ed.). Signal Processing, Sensor Fusion, and Target Recognition VI. Proceedings of SPIE. Vol. 3. pp. 182–193.

[2] Chandra, Kumar Pakki & Gu, Da-Wei. (2019). <a href="https://amzn.to/4dPedtG" onclick="fathom.trackEvent('UKF - Amazon - Chandra');">Nonlinear Filtering: Methods and Applications</a>. 10.1007/978-3-030-01797-2. 

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

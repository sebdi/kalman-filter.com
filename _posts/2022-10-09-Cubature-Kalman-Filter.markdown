---
layout: post
title:  "Cubature Kalman Filter"
description: The Cubature Kalman Filter (CKF) is the newest representative of the sigma-point methods and is based on the Cubature rule.
permalink: /cubature-kalman-filter/
date:   2023-01-29 00:00:00 +0000
description: The Cubature Kalman Filter (CKF) is the newest representative of the sigma-point methods.
categories: kalman-filter
---

The Cubature Kalman Filter (CKF) is the newest representative of the sigma-point methods.
The selection of sigma points in the CKF is slightly different from the Unscented Kalman Filter (UKF) and is based on the Cubature rule.
As for the UKF, the CKF follows the idea that it is easier to approximate a probability function than to linearize a nonlinear function.

Essentially, the challenge in nonlinear state estimation is that the involuted nonlinear functions result in, for example, previously Gaussian distributed variables no longer being Gaussian distributed. Mathematically speaking, the cause is that an integral must be computed over the nonlinear function which is not analytically possible for most cases. Since Kalman filters only consider the first two statistical moments, it is sufficient to obtain only the first two moments as long as they correspond to the first two moments of the true distribution function.

A method which is able to do this is the Curbatur rule which was derived by Arasaratnam and Haykin [1].

<h3>Cubature Rule</h3>
Explained in short words, the idea of the Cubature rule is to transform the state defined in Cartesian space into a spherical-radial coordinate system and to use the [Gaussian quadrature](https://en.wikipedia.org/wiki/Gaussian_quadrature) there.
It results in the following calculation rules for the sigma points.
For \\( i=1,2,...,n_x \\) with \\( \text{dim}( \mathbf{x} ) = n_x \\) the sigma points are

\\[ \mathcal{X}^{(i)}(k-1) = \mathbf{x}(k-1\|k-1) + \sqrt{n_x} \bigl(\mathbf{P}^{\frac{1}{2}}(k-1\|k-1)\bigl)_i \\]

and

\\[ \mathcal{X}^{(i+n_x)}(k-1) = \mathbf{x}(k-1\|k-1) - \sqrt{n_x} \bigl(\mathbf{P}^{\frac{1}{2}}(k-1\|k-1)\bigl)_i \\]

where the parenthesis \\( \bigl( \cdot \bigl)_i \\) indicates that the i-column of the matrix is to be used. 
The corresponding weight is computed with

\\[ W_i = \frac{1}{2n_x} .\\] 

Subsequently, the sigma points \\( \mathcal{X} \\) are used to be sent through the (nonlinear) function \\( \mathbf{f} \\) and by means of the weights \\( W_i \\) to calculate the first central moment

\\[ \mathbf{x}(k\|k-1) = \sum^{2n_x}_{i=1} \mathbf{f}(\mathcal{X}^{(i)}(k-1)) W_i  .\\]

The predicted covariance matrix of the predicted state results insignificantly different with additive addition of the process noise

\\[ \mathbf{P}(k\|k-1) = \sum^{2n_x}_{i=1} \bigl( \mathbf{f}(\mathcal{X}^{(i)}(k-1))-\mathcal{x}(k\|k-1) \bigl) \bigl( \mathbf{f}(\mathcal{X}^{(i)}(k-1))-\mathcal{x}(k\|k-1) \bigl)^T W_i + \mathbf{Q}_d(k-1)  .\\]

For the correction step, another set of sigma points must be calculated from the predicted state \\( \mathbf{x}(k\|k-1) \\) and the predicted covariance \\( \mathbf{P}(k\|k-1) \\),

\\[ \mathcal{X}^{(i)}(k) = \mathbf{x}(k\|k-1) + \sqrt{n_x} \bigl(\mathbf{P}^{\frac{1}{2}}(k\|k-1)\bigl)_i \\]

and

\\[ \mathcal{X}^{(i+n_x)}(k) = \mathbf{x}(k\|k-1) - \sqrt{n_x} \bigl(\mathbf{P}^{\frac{1}{2}}(k\|k-1)\bigl)_i  .\\]

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
Compared to the Unscented Transformation of the UKF, the Cubature rule has a great similarity and is even a special case of the UT method.
The advantage is that no parameters have to be tuned and since no negative weights are used there is no danger that negative definite covariance matrices are generated.
The disadvantage is that the larger n is, the wider the sigma points are spread than with the UKF, which leads to the fact that the outer regions of the used nonlinear function significantly influence the value of the sigma points. 

<h3>References</h3>
[1] I. Arasaratnam and S. Haykin, "Cubature Kalman Filters," in IEEE Transactions on Automatic Control, vol. 54, no. 6, pp. 1254-1269, June 2009, doi: 10.1109/TAC.2009.2019800.

[2] D. F. Crouse, "Cubature/unscented/sigma point Kalman filtering with angular measurement models," 2015 18th International Conference on Information Fusion (Fusion), 2015, pp. 1550-1557.

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

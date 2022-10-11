---
layout: post
title:  "Cubature Kalman Filter"
permalink: /cubature-kalman-filter/
date:   2022-10-11 00:00:00 +0000
categories: kalman-filter
---

The Cubature Kalman Filter (CKF) is the newest representative of the sigma-point methods.
The selection of sigma points in the CKF is slightly different from the Unscented Kalman Filter (UKF) and is based on the Cubature rule.
As for the UKF, the CKF follows the idea that it is easier to approximate a probability function than to linearize a nonlinear function.

<h3>Cubature Rule</h3>

For \\( i=1,2,...,n \\)

\\[ \mathcal{X}^{(i)}(k-1) = \mathbf{x}(k-1\|k-1) + \sqrt{n} \bigl(\mathbf{P}^{\frac{1}{2}}(k-1\|k-1)\bigl) \\]

\\[ \mathcal{X}^{(i+n)}(k-1) = \mathbf{x}(k-1\|k-1) - \sqrt{n} \bigl(\mathbf{P}^{\frac{1}{2}}(k-1\|k-1)\bigl) \\]

\\[ W_i = \frac{1}{2n} \\]

Predicted moments

\\[ \mathbf{x}(k\|k-1) = \sum^{2n}_{i=1} \mathbf{f}(\mathcal{X}^{(i)}(k-1)) W_i  \\]

\\[ \mathbf{P}(k\|k-1) = \sum^{2n}_{i=1} \bigl( \mathbf{f}(\mathcal{X}^{(i)}(k-1))-\mathcal{x}(k\|k-1) \bigl) \bigl( \mathbf{f}(\mathcal{X}^{(i)}(k-1))-\mathcal{x}(k\|k-1) \bigl)^T W_i + \mathbf{Q}_d(k-1)  \\]

Update step

\\[ \mathbf{y}(k\|k-1) = \sum^{2n}_{i=1} \mathbf{h}(\mathcal{X}^{(i)}(k)) W_i  \\]

\\[ \mathbf{P}(k\|k-1) = \sum^{2n}_{i=1} \bigl( \mathcal{X}^{(i)} - \mathbf{x}(k\|k-1)  \bigl) \bigl( \mathcal{X}^{(i)} - \mathbf{x}(k\|k-1)  \bigl)^T W_i  \\]

\\[ \mathbf{S}(k\|k-1) = \sum^{2n}_{i=1} \bigl( \mathbf{h}(\mathcal{X}^{(i)}(k))- \mathbf{y}(k\|k-1) \bigl) W_i + \mathbf{R}(k)  \\]
<h3>Conclusion</h3>
Compared to the Unscented Transformation of the UKF, the Cubature rule has a great similarity and is even a special case of the UT method.
The advantage is that no parameters have to be tuned and since no negative weights are used there is no danger that negative definite covariance matrices are generated.
The disadvantage is that the larger n is, the wider the sigma points are spread than with the UKF, which leads to the fact that the outer regions of the used nonlinear function significantly influence the value of the sigma points. 

<h3>References</h3>
I. Arasaratnam and S. Haykin, "Cubature Kalman Filters," in IEEE Transactions on Automatic Control, vol. 54, no. 6, pp. 1254-1269, June 2009, doi: 10.1109/TAC.2009.2019800.

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

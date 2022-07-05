---
layout: page
title: Kalman-Filter
permalink: /linear-kalman-filter/
---

<h4>State prediction</h4>

\\[ \mathbf{x}(k\|k-1) = \mathbf{A}_d(k-1) \mathbf{x}(k-1) + \mathbf{B}(k-1) \mathbf{u}(k-1) \\]


<h4>State prediction covariance</h4>

\\[ \mathbf{P}(k\|k-1) = \mathbf{A}_d(k-1) \mathbf{P}(k-1) \mathbf{A}^{T}_d(k-1) + \mathbf{Q}(k-1) \\]


<h4>State prediction covariance</h4>

\\[ \mathbf{K}(k) = \mathbf{P}(k\|k-1) \mathbf{C}^{T} (\mathbf{C} \mathbf{P}(k\|k-1) \mathbf{C}^{T} + \mathbf{R})^{-1} \\]


<h4>Update state covariance</h4>

\\[ \mathbf{x}(k) = \mathbf{x}(k\|k-1) + \mathbf{K}(k) (\mathbf{y}(k) - \mathbf{C} \mathbf{x}(k\|k-1) )  \\]

<h4>Update state estimate</h4>

\\[ \mathbf{K}(k) = (\mathbf{I} - \mathbf{K}(k) \mathbf{C} ) \mathbf{P}(k\|k-1) \\]


[jekyll-organization]: https://github.com/jekyll

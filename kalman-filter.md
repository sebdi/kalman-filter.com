---
layout: page
title: Kalman filter
permalink: /linear-kalman-filter/
---
The Kalman filter is an estimator to estimate the state of a dynamic linear system. 
This page uses the following notation and syntax for the representation of a linear stochastic system

\\[ \dot{\mathbf{x}}(t) = \mathbf{A}(t) \mathbf{x}(t) + \mathbf{B}(t) \mathbf{u}(t) + \mathbf{G}(t) \mathbf{z}(t) \\]

where \\( t \\) represents the time, \\( \mathbf{x}(t) \\) is the state vector, \\( \mathbf{u}(t) \\)is the control input, \\( \mathbf{z}(t) \\) is the input or process noise with \\(  \mathcal{N}(0,\mathbf{Q(t)})  \\) , \\( \mathbf{A}(t) \\) the system matrix, \\(  \mathbf{B}(t)  \\) input matrix and \\(  \mathbf{G}(t)  \\) process noise matrix.

The output of such a system, in general, can be written as follows

\\[ \mathbf{y}(t) = \mathbf{C}(t) \mathbf{x}(t) + \mathbf{v}(t) \\]

where \\( \mathbf{C}(t) \\) is the measurement matrix that projects the state-space in the measurement-space and \\( \mathbf{v}(t) \\) adds additive noise with \\(  \mathcal{N}(0,\mathbf{R(t)})  \\).

<h1>Discrete state-space</h1>
For the use and application of the Kalman filter within a computer, the discretized state space description is used. 
For this purpose, this page uses the following notation
\\[ \mathbf{x}(k) = \mathbf{A}_d(k-1) \mathbf{x}(k-1) + \mathbf{B}_d(k-1) \mathbf{u}(k-1) + \mathbf{G}_d(k) \mathbf{z}(k-1) \\]

\\[ \mathbf{y}(k) = \mathbf{C}(k) \mathbf{x}(k) + \mathbf{v}(k) . \\]

With this introduction and introduction of the notation, the Kalman filter equations can be hereafter presented.
<h1>Kalman filter</h1>
The equations of the Kalman filter consist of five major steps:
<h4>State prediction</h4>

\\[ \mathbf{x}(k\|k-1) = \mathbf{A}_d(k-1) \mathbf{x}(k-1) + \mathbf{B}_d(k-1) \mathbf{u}(k-1) \\]


<h4>State prediction covariance</h4>

\\[ \mathbf{P}(k\|k-1) = \mathbf{A}_d(k-1) \mathbf{P}(k-1) \mathbf{A}^{T}_d(k-1) + \mathbf{Q}_d(k-1) \\]


<h4>State prediction covariance</h4>

\\[ \mathbf{K}(k) = \mathbf{P}(k\|k-1) \mathbf{C}^{T}(k) (\mathbf{C}(k) \mathbf{P}(k\|k-1) \mathbf{C}^{T}(k) + \mathbf{R})^{-1} \\]


<h4>Update state covariance</h4>

\\[ \mathbf{x}(k) = \mathbf{x}(k\|k-1) + \mathbf{K}(k) (\mathbf{y}(k) - \mathbf{C}(k) \mathbf{x}(k\|k-1) )  \\]

<h4>Update state estimate</h4>

\\[ \mathbf{K}(k) = (\mathbf{I} - \mathbf{K}(k) \mathbf{C}(k) ) \mathbf{P}(k\|k-1) \\]

<h1>Further reading</h1>
The Kalman filter is named after the publication of Rudolf Emil Kalman who presented his approach in 1960 in the paper "A New Approach to Linear Filtering and Prediction Problems” [1]. However, the idea was described by other authors several years earlier. Nevertheless, the spread of the concept is attributed to Kalman.

[jekyll-organization]: https://github.com/jekyll

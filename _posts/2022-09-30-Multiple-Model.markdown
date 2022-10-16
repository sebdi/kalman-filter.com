---
layout: post
title:  "Multiple Model State Estimation"
permalink: /multiple-model-state-estimation/
date:   2022-10-17 00:00:00 +0000
categories: kalman-filter
---

Central for the performance of a model-based state estimator is how well the model corresponds to the process to be observed.
In practice, the question arises whether the process to be estimated always behaves exactly according to one model or whether the process changes between different models.
The solution is to take this into account by using multiple models.
In the literature this aspect is called multiple model.
The challenge is both how exactly, i.e. only one model or the mixture of several models, and according to which rules the process changes into the different models.

<h3>First generation</h3>
In the so-called first generation of MM methods, it is assumed that the system behaves according to a single model which is not known in advance. 
It is only known that it is one model out of a known set of models. 
Therefore, the approach is to use r parallel Kalman filters, i.e. one Kalman filter for each model. 
By considering the innovation covariance matrix, a recursive estimator can be derived, which over time gives a higher weighting to the Kalman filter for which the model used corresponds to the actual model.

<h3>Second generation</h3>
The second generation removes the need to stick to a specific one for the entire runtime and allows the system to switch between the different models. 
However, this freedom leads to the fact that r^n parallel filters are needed for the optimal result. 
In order to have practical solutions, several sub-optimal ones exist. 

<h3>Third generation</h3>
The third generation basically does not allow any of the previously mentioned restrictions, i.e. the system can also behave according to unknown models. 
However, solutions for this problem do not go quite so strictly.

<h3>Further reading</h3>
Probably the most comprehensive work on the subject is the paper by Ri and Jilkov, although it refers to the tracking use case [1].

For the IMM approach, S. Dingler's paper goes into more detail and describes the advantages and disadvantages [2].

<h3>References</h3>
[1] X. Rong Li and V. P. Jilkov, "Survey of maneuvering target tracking. Part V. Multiple-model methods," in IEEE Transactions on Aerospace and Electronic Systems, vol. 41, no. 4, pp. 1255-1321, Oct. 2005, doi: 10.1109/TAES.2005.1561886.

[2] Dingler, Sebastian. "State estimation with the Interacting Multiple Model (IMM) method." [arXiv preprint arXiv:2207.04875, 2022](https://arxiv.org/abs/2207.04875).

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

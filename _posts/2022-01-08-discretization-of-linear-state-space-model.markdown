---
layout: post
title:  "Discretization of linear state space model"
date:   2022-01-08 00:00:00 +0000
categories: kalman-filter matlab
---

In practice, the discretization of the equation of state is of particular importance.

For systems with high degrees of freedom (>2), the determination is not particularly trivial and presents even experienced individuals with considerable challenges due to the certain algebra.

The details of the derivation shall be omitted here because this is only suitable for the really interested reader and here we will deal with the very practical determination with the help of MATLAB.

For the zero-order hold (ZOH) discretization, the following lines give the vectors and matrices we are looking for:

In general, it is recommended to simplify the matrices in continuous time as much as possible before performing the discretization.

<h3>Formulas based on ZOH discretization</h3>

\\[ \mathbf{A}_d = e^{\mathbf{A} T_s} \\]


<h3>Computation in MATLAB</h3>
{% highlight matlab %}
Ad = expm(A*Ts);

syms tau
Bd = vpa(Ad*int(expm(-A*tau)*B,tau,[0 0.1]));

syms nu
Gd = vpa(int(expm(A*tau)*G,tau,[0 Ts]));
{% endhighlight %}

<h3>An elegant way</h3>

A particularly elegant way is to construct a matrix that yields the same result as described above.

\\[ e^{\begin{bmatrix}\mathbf{A} & \mathbf{B}\\\\ \mathbf{0} & \mathbf{0}\end{bmatrix} \cdot T_s} = \begin{bmatrix}\mathbf{A}_d & \mathbf{B}_d \\\\ \mathbf{0} & \mathbf{I}\end{bmatrix} \\]

For exampel for a system with \\( \mathbf{A}_{3 \times 3}, \mathbf{B}\_{1 \times 3} \\)

{% highlight matlab %}
big=expm([A B;zeros(1,4)]*Ts);
Ad=big(1:3,1:3);
bd=big(1:3,4);
{% endhighlight %}

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

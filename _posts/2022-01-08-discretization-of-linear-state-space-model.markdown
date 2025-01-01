---
layout: post
title:  "Discretization of linear state-space model based on ZOH discretization or Matrix Fraction Decomposition"
permalink: /discretization-of-linear-state-space-model/
date:   2024-01-08 00:00:00 +01000
categories: kalman-filter matlab
---

In practice, the discretization of the state-space equations is of particular importance.

For systems with high degrees of freedom (>2), the determination is not particularly trivial and presents even experienced individuals with considerable challenges due to the specialized algebra.

The details of the derivation shall be omitted here because this is only suitable for the really interested reader and here we will deal with the very practical determination with the help of MATLAB.

In general, it is recommended to simplify the matrices in continuous time as much as possible before performing the discretization.

<h3>Formulas based on ZOH discretization</h3>

\\[ \mathbf{A}_d = e^{\mathbf{A} T_s} \\]

\\[ \mathbf{B}_d = \int^{T_s}\_{0}  e^{\mathbf{A} \tau} d \tau  \mathbf{B}  \\]

\\[ \mathbf{G}_d = \int^{T_s}\_{0}  e^{\mathbf{A} \tau} \mathbf{G} d \tau  \\]

\\[ \mathbf{Q}_d = \int^{T_s}\_{0}  e^{\mathbf{A} \tau} \mathbf{Q} e^{\mathbf{A}^T \tau} d \tau  \\]

\\[ \mathbf{R}_d = \mathbf{R} \frac{1}{T_s} \\]

<h3>Computation in MATLAB</h3>
{% highlight matlab %}
Ad = expm(A*Ts);

syms tau
Bd = vpa(int(expm(A*tau)*B,tau,[0 Ts]));

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

<h3>Matrix Fraction Decomposition</h3>
In a similar way, \\( \mathbf{Q}_d \\) can also be calculated via a matrix exponential function that is called Matrix Fraction Decomposition [1]:

\\[ \begin{bmatrix}\mathbf{A}_d & \mathbf{Q}_d \bigl( \mathbf{A}^{T}_d \bigr)^{-1} \\\\ \mathbf{0} & \bigl( \mathbf{A}^{T}_d \bigr)^{-1}\end{bmatrix} = e^{ \begin{bmatrix}\mathbf{A} & \mathbf{G} \mathbf{Q} \mathbf{G}^T \\\\ \mathbf{0} & - \mathbf{A}^T\end{bmatrix} \cdot T_s} \\]

The matrix \\( \mathbf{Q}_d \\) can be computed via

\\[ \mathbf{Q}_d =  \mathbf{Q}_d \bigl( \mathbf{A}^{T}_d \bigr)^{-1} \mathbf{A}^{T}_d \\]

For exampel for a system with \\( \mathbf{A}_{3 \times 3}, \mathbf{G}\_{1 \times 3} \\)

{% highlight matlab %}
big=expm([A G*q*G;zeros(3) -A']*Ts);
Ad=big(1:3,1:3);
Qd=big(1:3,4:6) * Ad';
{% endhighlight %}

<h3>References</h3>

[1] Särkkä, S., & Solin, A. (2019). <a href="https://amzn.to/4auJSO6" onclick="fathom.trackEvent('Discretization - Amazon - Bar-Shalom');">Applied Stochastic Differential Equations (Institute of Mathematical Statistics Textbooks).</a> Cambridge: Cambridge University Press. doi:10.1017/9781108186735

[2] C. Van Loan, "Computing integrals involving the matrix exponential," in IEEE Transactions on Automatic Control, vol. 23, no. 3, pp. 395-404, June 1978, doi: 10.1109/TAC.1978.1101743.

<h3>Newsletter</h3>
<iframe src="https://embeds.beehiiv.com/29a6e516-926f-4340-80b5-8d0ce6c3198e" data-test-id="beehiiv-embed" width="100%" height="320" frameborder="0" scrolling="no" style="border-radius: 4px; border: 2px solid #e5e7eb; margin: 0; background-color: transparent;"></iframe>

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

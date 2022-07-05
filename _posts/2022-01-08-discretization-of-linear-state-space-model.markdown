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

{% highlight matlab %}
Ad = expm(Ts*A);

syms tau
Bd = vpa(Ad*int(expm(-A*tau)*B,tau,[0 0.1]));

syms nu
Gd = vpa(int(expm(A*tau)*G,tau,[0 Ts]));
{% endhighlight %}

A particularly elegant way is to construct a matrix that yields the same result as described above.

$$ x = y ^2 $$


[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

---
layout: post
title:  "Discretization of linear state space model"
date:   2022-01-08 00:00:00 +0000
categories: jekyll update
---

{% highlight matlab %}
simplify(int(expm(A*tau)*B,tau,[0 dT]))
{% endhighlight %}


[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

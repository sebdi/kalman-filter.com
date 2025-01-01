---
layout: post
title:  "Joseph form"
description: Learn about the Joseph form update equation, a more stable solution that minimizes errors and improves reliability, despite increased computation time.
permalink: /joseph-form/
date:   2024-06-01 00:00:00 +0000
categories: kalman-filter
---

One of the biggest challenges in implementing Kalman filters is that numerical errors can cause the covariances to lose symmetry and positive definiteness, causing the filter to diverge and no longer provide an estimation result.
This happens especially in system models with large degrees of freedom. 
The solution is to use a numerically more stable equation. 
For the update equation this is the so-called joseph form, which has a higher computation time, but is less sensitive to numerical errors.


\\[ \mathbf{P}(k\|k) = \bigl(\mathbf{I} - \mathbf{K}(k) \mathbf{C}(k) \bigl) \mathbf{P}(k) \bigl(\mathbf{I} - \mathbf{K}(k) \mathbf{C}(k) \bigl)^T + \mathbf{K}(k) \mathbf{R}(k) \mathbf{K}(k)^T \\]

<h3>Newsletter</h3>
<iframe src="https://embeds.beehiiv.com/29a6e516-926f-4340-80b5-8d0ce6c3198e" data-test-id="beehiiv-embed" width="100%" height="320" frameborder="0" scrolling="no" style="border-radius: 4px; border: 2px solid #e5e7eb; margin: 0; background-color: transparent;"></iframe>

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

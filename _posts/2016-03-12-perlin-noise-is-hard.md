---
layout: post
title: Perlin noise is hard
banner: Perlin Vol 1.
subbanner: Convincing noise is hard
---

If, like me, you have a tendency to try to reimplement every wheel you see, you may find that your Perlin noise doesn't look right.

I have 3 little nuggets of wisdom:

* First off, if you have any obvious discontinuities at your grid lines, like this, your interpolation formula is wrong; go fix it now.

[![Uggh, yuk]({{ site.url }}/img/blog/perlin/perlin1_terrible.png)]({{ site.url }}/img/blog/perlin/perlin1_terrible.png)

* However, if it looks like the below, it's not so bad.

[![Check out them gridlines]({{ site.url }}/img/blog/perlin/perlin1_fail.png)]({{ site.url }}/img/blog/perlin/perlin1_fail.png)

It looks weird, true, but it is a correct implementation of Perlin noise... 
The issue is that, while it is continuous (trust me, make the pixels big, and squint really hard), 
human visual sensitivity is [logarithmic](https://en.wikipedia.org/wiki/Weber%E2%80%93Fechner_law#Vision), 
so this level of smoothness (C0) isn't enough to look natural to our eyes. 

In other words, whether this is broken or not depends on your requirements
--- if you ultimately will use your noise function for something non-visual, 
you can rest here and pat yourself on the back. 
If you *really* need some grayscale that looks aesthetically pleasing, 
then you'll need a smoother blending function, cubic being the goto example.

Here's the same noise with cubic interpolation instead of linear:

[![Good]({{ site.url }}/img/blog/perlin/perlin1_success.png)]({{ site.url }}/img/blog/perlin/perlin1_success.png)

Visually... much nicer.

* On the topic of dumb things to do with Perlin noise, please don't use it in RGB space since the results *do* look *dumb*.

[![What is this even]({{ site.url }}/img/blog/perlin/perlin2_RGB.png)]({{ site.url }}/img/blog/perlin/perlin2_RGB.png)

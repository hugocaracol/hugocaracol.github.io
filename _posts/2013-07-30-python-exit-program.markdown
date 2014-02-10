---
layout: post
title: "python exit program"
date: 2013-07-30 19:38
comments: true
categories: [python]
---

One question that is asked a lot on the web is:
>How to exit a Python program

That is **very simple**. Just:

{% highlight python %}
import sys
sys.exit()
{% endhighlight %}

You can also pass as argument an exit status. Please check [python docs](http://docs.python.org/2/library/sys.html#sys.exit).

---
layout: post
title: "Shaker Sort"
tags:
- python
- sort
status: publish
published: true
categories: [algorithms]
---
This algorithm (AKA cocktail sort) has a complexity of O(n²).

The procedure is similar to the [bubble sort](http://neverpop.wordpress.com/2012/12/06/bubble-sort/). In fact, the only difference is that it sorts in both directions.

{% highlight python %}
def shaker_sort(l):
    n = len(l)
    swap_count = 1
    while swap_count > 0:
        swap_count = 0
        # forward
        for i in range(n - 1):
            if l[i] > l[i + 1]:
                l[i], l[i + 1] = l[i + 1], l[i]
                swap_count += 1
        # backwards
        for i in range(n - 1, 0, -1):
            if l[i] < l[i - 1]:
                l[i], l[i - 1] = l[i - 1], l[i]
                swap_count += 1
        n -= 1
{% endhighlight %}

As you can see from the code above, for each while loop there is a forward pass and a backwards pass. This fact turns this algorithm more efficient than bubble sort.

{% highlight python %}
l = [1, 32, 3, 45, 5, 6, 8, 0]
shaker_sort(l)
print l  # [0, 1, 3, 5, 6, 8, 32, 45]
{% endhighlight %}

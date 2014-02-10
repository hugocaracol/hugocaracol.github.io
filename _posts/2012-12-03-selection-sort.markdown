---
layout: post
title: "Selection Sort"
tags:
- python
- sort
status: publish
published: true
categories: [algorithms]
---
This sort algorithm has a complexity of O(n²).  
For each element in the array the algorithm tries to find the lowest value in the remaining elements of the array. If it finds that lowest value, a swap operation happens. If not, it does nothing and continues.
In the end we get an ordered array with less swaps than if we had done the sequential sort [1].

{% highlight python %}
def sel_sort(l):
    n = len(l)
    for i in range(0, n - 1):
        indMin = i
        for j in range(i + 1, n):
            if l[j] < l[indMin]:
                indMin = j
        if indMin != i:
            l[i], l[indMin] = l[indMin], l[i]
{% endhighlight %}

Usage:

{% highlight python %}
l = [10, 45, 4, 5, 2, 10, 1]
sel_sort(l)
print l  # [1, 2, 4, 5, 10, 10, 45]
{% endhighlight %}

[1] For each element, if the algorithm finds any lower value, it swaps those values.

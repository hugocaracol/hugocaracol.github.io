---
layout: post
title: "Insertion Sort"
tags:
- python
- sort
status: publish
published: true
categories: [algorithms]
---
The complexity of this algorithm varies from O(n²) and O(n) on the best case.

This algorithm assumes that the array is divided in two parts. The first one is sorted and the other is not. It begins with a "sorted" segment that contains only the element on the index 0. The elements from 1 to n -1(n being the length of the array) is assumed as not being sorted.

Each element we want to sort is copied to a temp variable and compared with the elements on the left side (sorted segment). Each left side value, if it's bigger than the one we want to sort, moves one place to the right, and so on, until we reach an "open" index where we store our element. Next element please!!! (loop) The algorithm finishes its job when it reaches the last element to be sorted and puts it on the right place (wherever it will be :-) ).

<!-- more -->

##Implementation

{% highlight python %}
def insertion_sort(l):
    n = len(l)
    for i in range(1, n):
        temp = l[i]
        j = i - 1
        while j >= 0 and l[j] > temp:
            l[j + 1] = l[j]
            j -= 1
        l[j + 1] = temp
{% endhighlight %}

{% highlight python %}
l = [1, 32, 3, 45, 5, 6, 8, 0, 99, 72]
insertion_sort(l)
print l  # [0, 1, 3, 5, 6, 8, 32, 45, 72, 99]
{% endhighlight %}

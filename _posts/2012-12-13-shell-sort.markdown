---
layout: post
title: "Shell Sort"
tags:
- python
- sort
status: publish
published: true
categories: [algorithms]
---
This algorithm is based on <a title="Insertion Sort" href="http://neverpop.wordpress.com/2012/12/11/insertion-sort/">Insertion Sort</a> but instead of starting the comparison between adjacent elements it introduces the notion of a gap.

On simple implementations the <a title="Gap Sequences" href="http://en.wikipedia.org/wiki/Shellsort#Gap_sequences">gap is just N/2</a> meaning that it starts comparing elements separated by N/2 elements. For example if we have an array with 10 elements, the first comparison will be between element on index the 5 and element on the index 0. If elements are not sorted an exchange occurrs. If they are, the algorithm moves to the next elements (on indexes 6 and 1). Notice that the gap of 5 continues until the end of the array. After that, the gap equals the value of the last gap divided by 2 again. New gap is 2 (5/2). Then we start again, comparing indexes 2 and 0, 3 and 1, 4 and 2, etc. Please notice that on this last comparison (4 and 2) if element on index 4 was smaller than the one on index 2 the algorithm would also compare the element with the element on the index 0 (keeping a gap of 2).

After a gap of 2 comes the gap of 1. After this last gap (of 1), we can say that the array is sorted.

I'll show you the code.
<!-- more -->

{% highlight python %}
def shell_sort(l):
    n = len(l)
    gap = n / 2
    while gap > 0:
        i = 0
        while gap + i < n:
            temp = l[gap + i]
            j = i
            while temp < l[j] and j >= 0:
                l[j + gap] = l[j]
                j -= gap
            l[j + gap] = temp
            i += 1
        gap /= 2
{% endhighlight %}

{% highlight python %}
l = [33, 32, 3, 45, 5, 6, 8, 0, 99, 72, 14]
shell_sort(l)
print l  # [0, 3, 5, 6, 8, 14, 32, 33, 45, 72, 99]
{% endhighlight %}

---
layout: post
title: "Binary Search"
tags:
- binary search
categories: [algorithms]
status: publish
type: post
published: true
---
The binary search is a divide and conquer search algorithm. In each step it reduces the size of the array to search, so we can say it has a O(log n) complexity.
When the algorithm starts, it finds the middle index of the array. Then the key we want to search is compared with this middle index. If we have a match we return the index. If the key has a lower value than the index key, it tries to search the key on the left part (lower values), otherwise it searches for the key on the right part (higher values). On the next step we have a much smaller "area" (half of what we had on the first step) to find the key we want.
This can be done with an iterative or recursive approach.
<!-- more -->

My implementation (Python) uses recursion to find a key in a array.

{% highlight python %}
def binarysearch2(l, pinit, pend, value):
    if pinit > pend:
        return -1

    m_idx = (pend + pinit) / 2

    if l[m_idx] == value:
        return m_idx

    if value > l[m_idx]:
        return binarysearch2(l, m_idx + 1, pend, value)
    else:
        return binarysearch2(l, pinit, m_idx - 1, value)
{% endhighlight %}

This algo first checks if the left bound index is bigger than the right bound one. If it is, the algo couldn't find a match and returns -1. If it's not it continues, determining the middle index regarding the left and right bounds. If this middle index value equals to the key we want to find, it returns the index. Otherwise it checks if the desired key is lower or bigger than the key stored in the middle index. Then the function is called again (recursion) with the left and right bounds updated.

Usage:

{% highlight python %}
l = [2, 5, 7, 10, 12]
# we search for the key in the entire array, so the left index=0 and
# the right index=(size of the array - 1)
print binarysearch2(l, 0, len(l) - 1, 10)  # matches on index 3
print binarysearch2(l, 0, len(l) - 1, 99)  # theres no match and -1 is returned
{% endhighlight %}

---
layout: post
title: "Bubble Sort"
tags:
- python
- sort
status: publish
published: true
categories: [algorithms]
---
This sorting algorithm has a complexity of O(n²).  
The algorithm compares each element in the array with the element in the adjacent position.
If the two elements are not sorted (element in the position i is bigger than the element in the position i+1), the algorithm swaps them.
This way the smaller elements come to the beginning of the array, while the bigger ones go to the end.
For each pass of the while loop the elements to sort are shorten by one, because the biggest element of that sub-array is put on the correct place.
The algorithm ends when a while loop pass causes no swap operations, meaning that the array is sorted.

{% highlight python %}
def bubble_sort(l):
    n = len(l)
    compar = 0
    swap_count = 1
    while swap_count > 0:
        swap_count = 0
        for i in range(n - 1):
            if l[i + 1] < l[i]:
                l[i], l[i + 1] = l[i + 1], l[i]
                swap_count += 1
        n -= 1
{% endhighlight %}

Example:

{% highlight python %}
l = [1, 32, 3, 45, 5, 8, 0, 6]
bubble_sort(l)
print l  # [0, 1, 3, 5, 6, 8, 32, 45]
{% endhighlight %}

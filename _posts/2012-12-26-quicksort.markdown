---
layout: post
title: "Quicksort"
tags:
- python
- sort
status: publish
published: true
categories: [algorithms]
---
The complexity of this algorithm is O(n log n). This is a divide and conquer algorithm. We take smaller sub-lists and sort them until every element in the list is in the right place.

We start by sorting the leftmost, the rightmost and the middle elements, putting those 3 elements in the right place. The median element stays in the middle assuming the function of pivot. After choosing the pivot (like a threshold), all elements below that element stay or move to the left and all elements above stay or move to the right. This operation is called partitioning. We then take the sub-list on the left and the sub-list on the right and do recursive calls until these sub-lists are sorted.
<!-- more -->
I use 2 functions:

* quicksort
* partitioning

##function quicksort

This function is called with 3 main arguments. The list to sort and 2 more arguments that determine the initial and ending indexes of the sub-list. It's in this function that we have the stop conditions of the recursive calls.
{% highlight python %}
def quicksort(l, pini, pend, debug=False):

    n = pend - pini + 1
    if n <= 1:
        if debug:
            print 'Sorted ' + str(l[pini:pend + 1])
        return
    if n == 2:
        if l[pini] > l[pend]:
            l[pend], l[pini] = l[pini], l[pend]
        if debug:
            print 'Sorted ' + str(l[pini:pend + 1])
        return
    m = (pend + pini) / 2  # pivot
    # sort 3 elements
    if l[pini] > l[m]:
        l[pini], l[m] = l[m], l[pini]
    if l[m] > l[pend]:
        l[m], l[pend] = l[pend], l[m]
    if l[pini] > l[m]:
        l[pini], l[m] = l[m], l[pini]
    if n == 3:
        if debug:
            print 'Sorted ' + str(l[pini:pend + 1])
        return

    ind_i = pini
    ind_j = pend

    m = partitioning(l, pini, m, pend, debug)
    if debug:
        print 'Call quicksort with Left sub-list: ' + str(l[pini: m])
    quicksort(l, pini, m - 1, debug)
    if debug:
        print 'Call quicksort with Right sub-list: ' + str(l[m + 1:pend + 1])
    quicksort(l, m + 1, pend, debug)
{% endhighlight %}



##function partitioning

This function is called with 4 main arguments. The list to sort and 3 more arguments that determine the initial, the middle and ending indexes of the sub-list. This is the function that puts the elements in the right place, below and above the pivot.

{% highlight python %}
def partitioning(l, pini, m, pend, debug=False):

    if debug:
        print 'Partitioning: ' + str(l[pini:pend + 1]) + \
            ' with pivot l[' + str(m) + ']=' + str(l[m])

    while True:
        i = pini
        while l[i] <= l[m] and i < m:
            i += 1
        j = pend
        while l[m] < l[j] and j > m:
            j -= 1
        if i == m and j == m:
            if debug:
                print 'Finished partitioning ' + str(l[pini:pend + 1])
            return m
        if i != m and j != m:
            l[i], l[j] = l[j], l[i]
        elif i == m:  # 1st half is sorted
            if debug:
                print '1st half (' + str(l[pini:m]) + ') is below pivot. ' + \
                    'Moving pivot to index ' + str(j)
            l[m], l[j] = l[j], l[m]
            m = j
            if debug:
                print 'Updated list ' + str(l)
        elif j == m:  # 2nd half is sorted
            if debug:
                print '2nd half (' + str(l[m + 1:pend + 1]) + ')is above ' + \
                    'pivot. Moving pivot to index ' + str(i)
            l[m], l[i] = l[i], l[m]
            m = i
            if debug:
                print 'Updated list ' + str(l)
{% endhighlight %}

##Running quicksort
The code above contains a lot of verbose text which is useful when understanding how this algorithm works. Let's run a quicksort with debug mode set to True.

{% highlight python %}
l = [7, 2, 8, 3, 6, 4, 1]
print 'unsorted: ' + str(l)
quicksort(l, 0, len(l) - 1, True)
print 'sorted: ' + str(l)
{% endhighlight %}

Output

{% highlight python %}
unsorted: [7, 2, 8, 3, 6, 4, 1]
Partitioning: [1, 2, 8, 3, 6, 4, 7] with pivot l[3]=3
2nd half ([6, 4, 7])is above pivot. Moving pivot to index 2
Updated list [1, 2, 3, 8, 6, 4, 7]
Finished partitioning [1, 2, 3, 8, 6, 4, 7]
Call quicksort with Left sub-list: [1, 2]
Sorted [1, 2]
Call quicksort with Right sub-list: [8, 6, 4, 7]
Partitioning: [6, 7, 4, 8] with pivot l[4]=7
1st half ([6]) is below pivot. Moving pivot to index 5
Updated list [1, 2, 3, 6, 4, 7, 8]
Finished partitioning [6, 4, 7, 8]
Call quicksort with Left sub-list: [6, 4]
Sorted [4, 6]
Call quicksort with Right sub-list: [8]
Sorted [8]
sorted: [1, 2, 3, 4, 6, 7, 8]
{% endhighlight %}

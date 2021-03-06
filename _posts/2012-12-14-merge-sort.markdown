---
layout: post
title: "Merge Sort"
tags:
- python
- sort
status: publish
published: true
categories: [algorithms]
---
The complexity of this algorithm is O(n log n).

The list of elements is recursively divided into smaller ones, each time cutting the size by half until we have sub-lists of only 1 element. Then starts the merging. These sub-lists are sorted and merged until we have 1 big sub-list the size of the original list. When we reach this point, the list is sorted.

In the implementation of this algorithm we use 2 functions. One for dividing and  one for merging. The function in charge of dividing is called recursively until we have lists of 1 element. This division of the list is always by half. The other function has the responsability of sorting and merging the sub-lists.
<!-- more -->
##Functions
###merge_sort
{% highlight python %}
def merge_sort(l, pini, pend, debug=False):
    if pini < pend:
        middle = (pini + pend) / 2

        if debug:
            print '1-invoking merge_sort with (' + str(pini) + ',' + \
                str(middle) + '):' + str(l[pini:middle + 1])
        merge_sort(l, pini, middle, debug)
        if debug:
            print '2-invoking merge_sort with (' + str(middle + 1) + \
                ',' + str(pend) + '):' + str(l[middle + 1:pend + 1])
        merge_sort(l, middle + 1, pend, debug)

        merge_list_sort(l, pini, middle, pend, debug)
{% endhighlight %}

This function is called with the list, and 2 indexes (start and end). On the line 3 the middle of the list is found. This is information is necessary so we can cut the list by half. On the line 8 this function is called recursively with the first half and on the line 12, the other half. After that we call the merging function (merge_list_sort).
###merge_list_sort
{% highlight python %}
def merge_list_sort(l, pini, middle, pend, debug=False):
    if debug:
        print 'invoking merge_list_sort with (' + str(pini) + ',' + \
            str(middle) + ',' + str(pend) + '):' + str(l[pini:pend + 1])
        print 'comparing: ' + str(l[pini:middle + 1]) + ' with ' + \
            str(l[middle + 1:pend + 1])

    new_list = []
    i = 0
    j = pini
    k = middle + 1
    while k <= pend and j <= middle:
        if l[k] < l[j]:
            new_list.append(l[k])
            k += 1
        else:
            new_list.append(l[j])
            j += 1

    while k <= pend:
        new_list.append(l[k])
        k += 1

    while j <= middle:
        new_list.append(l[j])
        j += 1

    if debug:
        print 'sorted: ' + str(new_list)

    #updates the l list with the values from the sorted list
    i = pini
    for item in new_list:
        l[i] = item
        i += 1
{% endhighlight %}

This function is called with the list, and 3 indexes (start,middle and end). We start by creating an empty list which will store the merged list (sorted). The while loop on line 12 compares elements from the two lists (in fact, the list is the same but conceptually we have 2 lists), sorting the elements and adding them to the list we created in this function. The two following while loops (lines 20 and 24) are used to add the remaining elements when the lists differ in size. In the end, on line 33, the for loop copies the elements of the new list to the original list passed by argument.

##Running

Next I'll show you some debug text created while running these functions:
###Run

{% highlight python %}
l = [6, 1, 3, 1, 8, 7, 24, 99]
print 'unsorted list -> ' + str(l)
merge_sort(l, 0, len(l) - 1, True)
print 'sorted list -> ' + str(l)
{% endhighlight %}

###Output

{% highlight python %}
unsorted list -> [6, 1, 3, 1, 8, 7, 24, 99]
1-invoking merge_sort with (0,3):[6, 1, 3, 1]
1-invoking merge_sort with (0,1):[6, 1]
1-invoking merge_sort with (0,0):[6]
2-invoking merge_sort with (1,1):[1]
invoking merge_list_sort with (0,0,1):[6, 1]
comparing: [6] with [1]
sorted: [1, 6]
2-invoking merge_sort with (2,3):[3, 1]
1-invoking merge_sort with (2,2):[3]
2-invoking merge_sort with (3,3):[1]
invoking merge_list_sort with (2,2,3):[3, 1]
comparing: [3] with [1]
sorted: [1, 3]
invoking merge_list_sort with (0,1,3):[1, 6, 1, 3]
comparing: [1, 6] with [1, 3]
sorted: [1, 1, 3, 6]
2-invoking merge_sort with (4,7):[8, 7, 24, 99]
1-invoking merge_sort with (4,5):[8, 7]
1-invoking merge_sort with (4,4):[8]
2-invoking merge_sort with (5,5):[7]
invoking merge_list_sort with (4,4,5):[8, 7]
comparing: [8] with [7]
sorted: [7, 8]
2-invoking merge_sort with (6,7):[24, 99]
1-invoking merge_sort with (6,6):[24]
2-invoking merge_sort with (7,7):[99]
invoking merge_list_sort with (6,6,7):[24, 99]
comparing: [24] with [99]
sorted: [24, 99]
invoking merge_list_sort with (4,5,7):[7, 8, 24, 99]
comparing: [7, 8] with [24, 99]
sorted: [7, 8, 24, 99]
invoking merge_list_sort with (0,3,7):[1, 1, 3, 6, 7, 8, 24, 99]
comparing: [1, 1, 3, 6] with [7, 8, 24, 99]
sorted: [1, 1, 3, 6, 7, 8, 24, 99]
sorted list -> [1, 1, 3, 6, 7, 8, 24, 99]
{% endhighlight %}

---
layout: post
title: "Preconditions, Postconditions and Invariants"
date: 2013-02-11 18:56
comments: true
categories: [debugging]
---
Frequently when debugging software we usually spend lots of time figuring out where exactly is the bug that is creating the
 failure of the software. Maybe it’s where the failure occurs but probably it’s not. Bugs can infect parts of software and
  these parts can cause failure of other parts. Sometimes finding a bug is like finding a needle in a haystack.

These 3 topics belong to the notion of design-by-contract (DbC) programming.
 Using this approach to design software, programmers should define code contracts - conditions
 that hold true before a method is run (**preconditions**), or after the method is run (**postconditions**) or both (**invariants**).
  Eiffel programming language was built around this concept (among others).

<!-- more -->

###Preconditions
Set of conditions that must assert to True before running the code in a method. These conditions can be, for example, variables that must
 have a positive value in order to the code in the method behave correctly.

{% highlight python %}
def square_root(number):
    assert number >= 0
    ...
{% endhighlight %}

###Postconditions
Set of conditions that must assert to True after the code in a method is run. These conditions verify the correct execution of the method code.

{% highlight python %}
def square_root(number):
    ...
    # variable root holds the square root value of number
    assert root * root == number  # i'm ignoring precision for readability purposes
    return root
{% endhighlight %}


###Invariants
Set of conditions that must assert to True before and after the execution of a method. Invariants are composed by a set of conditions that are always valid. This is
 useful to ensure that the code is running according to the algorithm.

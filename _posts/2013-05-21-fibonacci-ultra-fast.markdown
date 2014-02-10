---
layout: post
title: "Fibonacci ultra fast"
date: 2013-05-21 23:27
comments: true
categories: [code]
---
It's never too late to optimize the code. Never too late to realize there's a better approach.

After writing the last post I came across a different approach. If we compute the fibonacci number (n - 1),
that computing also computes the fib number (n - 2). Therefore we don't need to compute ir again. 

<!-- more -->

Look at the code. Now we use 2 functions.
One function that is called only once and another one that's called iteratively.

{% highlight python %}
def fib_fast_ultra(n):
    global fib_dict
    # clears global dictionary
    fib_dict.clear()
    n_1 = _fib_fast_ultra_iter(n-1)
    n_2 = fib_dict[n-2]
    return n_1 + n_2

def _fib_fast_ultra_iter(n):
    global fib_dict
    if n < 2:
        return n
    else:
        if (n-1) in fib_dict:
            n_1 = fib_dict[n - 1]
        else:
            n_1 = _fib_fast_ultra_iter(n - 1)
            fib_dict[(n - 1)] = n_1
            
        if (n - 2) in fib_dict:
            n_2 = fib_dict[n - 2]
        else:
            n_2 = _fib_fast_ultra_iter(n - 2)
            fib_dict[(n - 2)] = n_2

        return n_1 + n_2
{% endhighlight %}

Running the function above and the other shown [here](http://hugocaracol.github.io/blog/2013/05/21/accelerating-with-memoization) we see the magic! 
Computing the fibonacci number 500 using the function from the previous post
we get 0.0014700890 seconds. But if we run it with this function we get 0.0011551380 seconds. That is an **increasing performance of 78.6% improvement.**





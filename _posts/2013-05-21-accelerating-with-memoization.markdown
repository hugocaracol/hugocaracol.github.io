---
layout: post
title: "Accelerating with memoization"
date: 2013-05-21 20:20
comments: true
categories: [code]
---
Memoization can be incredible useful for expensive calculations. Just look at the results at the bottom of the post to check how long the 2 functions (with and without memoization) take to run the computation of fib 35.

<!-- more -->

{% highlight python %}
import time

def fib_slow(n):
    if n < 2:
        return n
    else:
        n_1 = fib_slow(n - 1)
        n_2 = fib_slow(n - 2)
        return n_1 + n_2
        

fib_dict = {}

def fib_fast(n):
    global fib_dict
    if n < 2:
        return n
    else:
        if (n-1) in fib_dict:
            n_1 = fib_dict[n - 1]
        else:
            n_1 = fib_fast(n - 1)
            fib_dict[(n - 1)] = n_1
            
        if (n - 2) in fib_dict:
            n_2 = fib_dict[n - 2]
        else:
            n_2 = fib_fast(n - 2)
            fib_dict[(n - 2)] = n_2

        return n_1 + n_2
{% endhighlight %}

Running...

{% highlight python %}
n = 35
print "Calculating fib of " + str(n)
print "Using fib_slow (without memoization)"
start_time = time.time()
print fib_slow(n)
print time.time() - start_time, "seconds"
print "Accelerating... (with memoization)"
start_time = time.time()
print fib_fast(n)
print '{0:.10f}'.format(time.time() - start_time), "seconds"
{% endhighlight %}

we get...

Calculating fib of 35  
Using fib_slow (without memoization)  
9227465  
11.9559807777 seconds  
Accelerating... (with memoization)  
9227465  
**0.0000698566 seconds**


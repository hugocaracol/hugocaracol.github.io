---
layout: post
title: "Decorators in Python"
date: 2013-01-08 20:56
comments: true
categories: [python]
---
Today I'll turn you into a decorator. Let's build a house and decorated it.
First things first... the house. In my world it's very simple to build a house, just call the function build_house().
After the "construction", in order to live there, we need to turn it cozier. For this effect we can call a decorator to shine things up.
When she arrives and sees the house she says: I'm going to decorate it with paitings on the wall. With no further delay she: @decorate("paitings").

{% highlight python %}
def decorate(thing):
    def _decorate(func_name):
        def __decorate(*args, **kwargs):
            ret = func_name(*args, **kwargs)
            if thing is not None:
                ret += " decorated with %s on the wall" % thing
            return ret
        return __decorate
    return _decorate 

@decorate("paintings")
def build_house():
    return "This is a house"
{% endhighlight %}

{% highlight python %}
>>> print build_house()
This is a house decorated with paintings on the wall
{% endhighlight %}
<!-- more -->
##Basic Usage
Decorators are used to change a function behaviour not by altering its code but by trapping the entry and exit points. Suppose you want to log the execution time of a function or make it return different values according to additional rules. The code to do that fits perfectly in a decorator.
## Explanation
When a function is decorated, it is run inside the decorator. This way, it is granted to the decorator the power to do the things described earlier.
###Functions return functions
Functions are first class objects. That means a function is indeed an object. Objects can be passed as arguments and be returned by functions. Therefore, functions can consume functions and return functions (replace the word functions with objects and you get a true statement as well).

The statement *functions can return functions* implies that an argument of a function can be a function and/or a function can be defined inside a function.

I'm getting sick of writing the word *function* but let's continue with a piece of code.

{% highlight python %}
def outer():
    def inner():
        print 'inner'
    return inner

fn = outer()
fn()  # prints inner
{% endhighlight %}
The code above defines the function inner inside outer. When we run outer(), the fn variable becomes equal to the function inner. 

>The line 4 returns the inner function without calling it (did you notice the lack of parentheses?).

Now if we call fn by adding parentheses () we get the execution of inner.

This leads us to...

###Closures
From Wikipedia...
>A closure—unlike a plain function pointer—allows a function to access those non-local variables even when invoked outside of its immediate lexical scope.

This means that our inner function can "remember" non-local variables, in our case, variables defined in outer function.

{% highlight python %}
def outer():
    x = 1
    def inner():
        print 'Got value %i from non-local' % x
    return inner
    
fn = outer()
del outer  # deletes object outer
fn()  # 'Got value 1 from non-local'
{% endhighlight %}

Even deleting the function outer, fn() still prints x variable.

###Functions as arguments

{% highlight python %}
def run_me():
    print "I'm running baby"

def outer(func):
    def inner():
        print "Before running the function"
        func()
    return inner
    
r = outer(run_me)
del run_me  # deletes run_me function
r()  # "I'm running baby"
{% endhighlight %}
On line 9, r gets function inner, returned by outer, which calls run_me (closure), which prints "I'm running baby".

###Decorate me!
Now let's try some decorator magic.

{% highlight python %}
def outer(func):
    def inner():
        print "Before running the function"
        func()
    return inner

@outer
def run_me():
    print "I'm running baby"

run_me()  #call run_me and prints the next 2 lines
Before running the function
I'm running baby
{% endhighlight %}

This is a simple example of a decorator. To apply the decorator to a function you use @ symbol followed by the decorator name. Here you no longer need to use 'r = outer(run_me)' and 'r()'. Much simpler!

How about if we change the definition of function run_me and add a parameter name to it? Will the decorator still works? Let's try!
{% highlight python %}
def outer(func):
    def inner():
        print "Before running the function"
        func()
    return inner

@outer
def run_me(name):
    print "I'm running %s" % name

run_me("Hugo")  # error below
TypeError: inner() takes no arguments (1 given)
{% endhighlight %}

We get an error saying function inner takes no arguments but we tried to pass the name "Hugo". Sadly it doesn't work. Happily we know how to fix it :)

To fix it we add a parameter 'name' to the function inner and then add the argument to the function call on line 4 below.
{% highlight python %}
def outer(func):
    def inner(name):
        print "Before running the function"
        func(name)
    return inner

@outer
def run_me(name):
    print "I'm running %s" % name

run_me("Hugo")  # now it works
#Before running the function
#I'm running hugo
{% endhighlight %}

Ok, so far so good. But what happens if I change the definition of the function run_me and add some more parameters? It will FAIL! Let's make the decorator more generic so we don't have to worry too much.

###*args and **kwargs

*args and **kwargs are conventional ways to get the arguments passed to a function.

For more info on *args and **kwargs please refer to one of the many posted questions on [Stackoverflow](http://stackoverflow.com/questions/3394835/args-and-kwargs) or [Python docs](http://docs.python.org/2/tutorial/controlflow.html#arbitrary-argument-lists).

As we want to abstract from the number of arguments passed to the function inner we replace 'def inner(name):' with 'def inner(\*args, \*\*kwargs):' and 'func(name)' with 'func(\*args, \*\*kwargs)'. 
{% highlight python %}
def outer(func):
    def inner(*args, **kwargs):
        print "Before running the function"
        func(*args, **kwargs)
    return inner

@outer
def run_me(name):
    print "I'm running %s" % name

run_me("Hugo")  # now it works
#Before running the function
#I'm running hugo
{% endhighlight %}
###Final touch - passing arguments to the decorator
This step is only required if you want to add some salt to the decorator, changing the way it works when certain arguments are passed to it. A lot can be "decorated" without this step.

The decorator function now has a parameter that indicates if we run it in debug mode or not. To get this, we add another "level" of functions. As you can see from the code below the decorator is composed by the function outer (with takes the debug argument), the function _outer (which takes the 'function' argument) and the function inner (which takes the arguments to the function to be decorated).

{% highlight python %}
def outer(debug=False):
    def _outer(func):
        def inner(*args, **kwargs):
            if debug:
                print "Before running the function"
            func(*args, **kwargs)
        return inner
    return _outer

@outer(False)  # decorated is NOT in debug mode
def run_me(name):
    print "I'm running %s" % name

run_me("Hugo")
# I'm running Hugo
{% endhighlight %}

This concludes the post.

####Disclaimer
Much of the ideas described here came from Simeon Franklin's [post on decorators](http://simeonfranklin.com/blog/2012/jul/1/python-decorators-in-12-steps/).
This post is available at <a href="http://www.codeproject.com/script/Articles/BlogFeedList.aspx?amid=10549383" rel="tag">CodeProject</a>.

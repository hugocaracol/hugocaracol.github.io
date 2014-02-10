---
layout: post
title: "Class inheritance check with __mro__"
date: 2013-01-12 13:41
comments: true
categories: [python]
---
Consider the following code:

{% highlight python %}
class A(object):
    pass
class B(A):
    pass
class C(B):
    pass
class D(C):
    pass
{% endhighlight %}
As you can see, D derives from C, which derives from B, which derives from A, which derives from object. In this example is very clear to see the relations between the classes. But usually, things get more complicated and you can't figure out in a second how the classes fit together. Specially if there's no documentation!

**MRO** stands for **Method Resolution Order** and defines the path to follow when calling methods belonging to super classes.

<!-- more -->

Use \<your_class\>.\_\_mro\_\_ to print a tuple which describes the order of the method resolution.

Let's print out D.\_\_mro\_\_

{% highlight python %}
>>> D.__mro__  # try also: [x.__name__ for x in D.__mro__]
(<class '__main__.D'>, <class '__main__.C'>, <class '__main__.B'>, <class '__main__.A'>, <type 'object'>)
{% endhighlight %}

How about if we change the defintion of D.

{% highlight python %}
class A(object):
    pass

class B(A):
    pass

class C(B):
    pass

class D(C, B):
    pass
{% endhighlight %}

{% highlight python %}
>>> D.__mro__
(<class '__main__.D'>, <class '__main__.C'>, <class '__main__.B'>, <class '__main__.A'>, <type 'object'>)
{% endhighlight %}

Python first looks in D, then in C, then in B, then in A and then in object.

But beware! Change the definition of D - *class D(B, C):* - and check its \_\_mro\_\_.

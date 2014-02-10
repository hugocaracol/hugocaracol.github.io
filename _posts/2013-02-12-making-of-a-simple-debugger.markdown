---
layout: post
title: "Making of a simple debugger"
date: 2013-02-12 23:50
comments: true
categories: [debugging]
---
I was introduced to sys.settrace in the Udacity course on debugging. Python docs say:
>Set the system’s trace function, which allows you to implement a Python source code debugger in Python. 

With this function, is very easy to make a simple debugger. Even if not implemented to use in a production environment, 
it's still a funny thing to do.

To make use of this, a trace function must be created. A trace function has 3 arguments: 
*frame*, *event* and *arg*. *frame* represents the current stack. *event* is a string like 
'call', 'line', 'return', 'exception' and some others. *arg* is used when event is 'return'. 

<!-- more -->

###Events 

-  'call' - triggered when a function is called
-  'line' - triggered when the interpreter is about to execute a new line of code
-  'return' - triggered when a function is about to return. The return value is stored in *arg*.
-  'exception' - triggered when an exception occurrs. A tuple like (exception, value, traceback) is stored in *arg*.

In the example that follows I use the first 3 events.

###Debugger

{% highlight python %}
class Debugger:

    def __init__(self):
        self.prev_vars = {}        

    def _get_changed_vars(self, f_locals):
        changed_vars = {}
        for k, v in f_locals.iteritems():
            if k in self.prev_vars:
                if v != self.prev_vars[k]:
                    changed_vars[k] = v
            else:
                changed_vars[k] = v
            # updates self.vars to the dictionary of current variable
            self.prev_vars[k] = v
        return changed_vars

    def track(self, frame, event, arg):
        prefix = "[DEBUG] "
        if event == 'line':
            print prefix + "Line no: %s" % frame.f_lineno
            changed_vars = self._get_changed_vars(frame.f_locals)
            if changed_vars:
                print prefix + "Changed variables: %s " % changed_vars
        elif event == 'call':
            print prefix + "Called: %s" % frame.f_code.co_name
        elif event == 'return':
            print prefix + "Exiting: %s" % frame.f_code.co_name
            print prefix + "  Returning: %s" % arg

        # for every event, the trace function is returned
        return self.track
{% endhighlight %}

I've implemented a class because I wanted to track the value change of variables (just the function *track* alone should be enough to get the gist of it).  
The **track** function does all the magic. As you can see, it has the 3 arguments pointed before.  
When some event occurrs this function is triggered. We watch for the event and act accordingly, printing variables, lines or return values. 
**This function must return itself** in order to keep the trace going.
I have not implemented breakpoints but with the line numbers it's a very easy thing to do.
**_get_changed_vars** function is just a helper function I use to get the new or updated variables.

Now, to use our shiny debugger we create a new instance of our class and pass *debugger.track* as a parameter to *settrace*. Then we invoke the code 
we want to debug and in the end we disable *settrace*.

{% highlight python %}
debugger = Debugger()

sys.settrace(debugger.track)
# call your function here
sys.settrace(None)
{% endhighlight %}

This debugger can be extended to infer invariants (one of the exercises on the debugging course). Watching how variables change, can give us hints about 
their ranges (min, max). Use it but don't take it too seriously ;-)

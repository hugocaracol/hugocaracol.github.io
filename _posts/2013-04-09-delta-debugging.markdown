---
layout: post
title: "Delta Debugging"
date: 2013-04-09 23:31
comments: true
categories: [debugging]
---
Delta Debugging is an automated debugging approach based on systematic testing.

Programmers follow this approach when they are manually debugging a piece of code that is failing at some point. Imagine some large input is causing a function to crash. What do you do? You start by reducing the input and check if the function works. If not, you do the chopping again until it works.  
This can be very hard to do if some random set from a 5000 character string is failing. It's like the needle in a haystack problem.

<!-- more -->

Well, delta debugging simplifies this A LOT! The algorithm **starts by chopping the input in 2** (the granularity - n). Then, it removes the first substring and tests with the second one. If the test passes, it runs now with only the first substring. The latter caused the test to fail. Good. Now we know that the problem is in there. The algorithm goes on using that substring (the one that failed the test) and, again, chopps it in two. But on the next step, both parts of the *failed substring* pass the test. When this occurrs the granularity value (n) is **updated to n * 2** and the algorithm continues until it finds the minimal input that is causing the failure. This is a very simple overview of how delta debugging works.

One importart part of this equation is the test function. We need a good function in order to get good results. You can create a generic test function that executes some code and traps exceptions. This way the test function can return true or false or even PASS or FAIL. Wichever suits you.

Below I give you an example of this code (provided by Professor Andreas Zeller). The test function checks wether the html select tag is found in the input string. It is obvious in this case but in practice you don't know what is causing the problem.


{% highlight python %}
import re

def test(s):
    global counter
    counter += 1
    if re.search("<SELECT[^>]*>", s) >= 0:
        print "test " + str(counter) + ": ", s, "FAIL"
        return "FAIL"
    else:
        print "test " + str(counter) + ": ", s, "PASS"
        return "PASS"


def ddmin(s):
    assert test(s) == "FAIL"

    n = 2     # Initial granularity
    while len(s) >= 2:
        print "Using n=", n
        start = 0
        subset_length = len(s) / n
        print "Using subset_length=", subset_length
        some_complement_is_failing = False

        while start < len(s):
            complement = s[:start] + s[start + subset_length:]

            if test(complement) == "FAIL":
                s = complement
                n = max(n - 1, 2)
                some_complement_is_failing = True
                break
                
            start += subset_length

        if not some_complement_is_failing:
            if n == len(s):
                break
            n = min(2*n, len(s))

    return s
{% endhighlight %}

{% highlight python %}
html_input = '<SELECT>foo</SELECT>'
counter = 0
print ddmin(html_input)
print 'called test ' + str(counter) + ' times'
{% endhighlight %}

{% highlight python %}
test 1:  <SELECT>foo</SELECT> FAIL
Using n= 2
Using subset_length= 10
test 2:  o</SELECT> PASS
test 3:  <SELECT>fo FAIL
Using n= 2
Using subset_length= 5
test 4:  CT>fo PASS
test 5:  <SELE PASS
Using n= 4
Using subset_length= 2
test 6:  ELECT>fo PASS
test 7:  <SECT>fo PASS
test 8:  <SELT>fo PASS
test 9:  <SELECfo PASS
test 10:  <SELECT> FAIL
Using n= 3
Using subset_length= 2
test 11:  ELECT> PASS
test 12:  <SECT> PASS
test 13:  <SELT> PASS
test 14:  <SELEC PASS
Using n= 6
Using subset_length= 1
test 15:  SELECT> PASS
test 16:  <ELECT> PASS
test 17:  <SLECT> PASS
test 18:  <SEECT> PASS
test 19:  <SELCT> PASS
test 20:  <SELET> PASS
test 21:  <SELEC> PASS
test 22:  <SELECT PASS
Using n= 8
Using subset_length= 1
test 23:  SELECT> PASS
test 24:  <ELECT> PASS
test 25:  <SLECT> PASS
test 26:  <SEECT> PASS
test 27:  <SELCT> PASS
test 28:  <SELET> PASS
test 29:  <SELEC> PASS
test 30:  <SELECT PASS
<SELECT>
called test 30 times
{% endhighlight %}

For the people using **git** there's **git bisect**. Some people say that:  
>"git bisect" is one of the most useful and overlooked debugging tools.




---
layout: post
title: "Code coverage"
date: 2013-02-04 22:42
comments: true
categories: [testing]
---
Code coverage is a metric used in software testing that describes the percentage of code that's been tested.
Some types of code coverage are: function coverage, statement coverage, branch coverage, loop coverage, among others.

###Function coverage  
It gives us a percentage of the functions being called by our tests. If we have 100 functions but only 50 are triggered by our test suite, 
the function coverage is 50%. This score can be improved by modifying our tests so that every function is executed at least once. The function 
coverage is also good to inform us if we have "dead functions" (functions that never get to be executed) in our code.

###Statement coverage
As the name says, it gives us the percentage of statements being covered by our tests. With this metric we can evaluate if every line of code is running.
 Well, this is not entirely true, as Line coverage is a metric by itself. If a statement is composed by several lines, you can guess the difference 
 between line and statement coverage.

###Branch coverage
This type of coverage evaluates if all branches are executed. If we have an *if* statement without the *else* clause, then branch coverage would never be 
100% because the code is missing the *else* clause. On the other hand if we have one *if then else* statement, the branch coverage would be equal to the
 statement coverage.

###Loop coverage
This metric can be important because bugs are commonly found in while loop boundary conditions. It specifies that we execute each loop 0 times, once, 
and more than once.

----

There are many [more metrics out there](http://www.kaner.com/pdfs/negligence_and_testing_coverage.pdf), including [MC/DC](http://en.wikipedia.org/wiki/Modified_condition/decision_coverage)
 (Modified Condition/Decision Coverage) used heavily in avionics software.
 
>Having a good coverage score is good but it's only an indicator that our tests are running all the code, not that the code does everything 
according to the specs.

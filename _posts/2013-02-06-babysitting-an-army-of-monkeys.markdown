---
layout: post
title: "Babysitting an army of monkeys"
date: 2013-02-06 23:19
comments: true
categories: [testing]
---
"Babysitting an Army of Monkeys: An analysis of fuzzing 4 products with 5 lines of Python" was the title of a talk by Charlie Miller in CodenomiCON 2010.  
Mr Miller talked about how he broke some software products with only just a few lines of Python code.
The steps are:

1. load a file into a buffer
2. at a random position of the buffer change the byte to a random one (5 lines of python code)
3. save the buffer
4. run the process
5. look at the exit code
6. if it doesn't die (no bug found)
7. start over

I embedded the youtube videos (Part1 and Part2). Click "Read On"!
<!-- more -->
<iframe width="560" height="315" src="http://www.youtube.com/embed/Xnwodi2CBws" frameborder="0" allowfullscreen></iframe>


<iframe width="560" height="315" src="http://www.youtube.com/embed/lK5fgCvS2N4" frameborder="0" allowfullscreen></iframe>

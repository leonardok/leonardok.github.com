---
layout: post
title: How to profile you python code
tags: [profiling, python, kcachegrind, cProfile]
description: Profile your python code to see where are the bottlenecks
last_updated: 06/10/2013
---

This is a brief explanation of how you can profile your code, in order to improve your benchmarks.

### Requirements

    pip install pyprof2calltree
    
    
### Who is taking my cheese?

There are lots of stuff that will try to eat your CPU time. You are programming these things, and you are using
things others did, all trying to eat out your CPU time. You need to find out where they are.


### Tools I use

The Python docs have a section about profilers, you should read it at 
[http://docs.python.org/2/library/profile.html](http://docs.python.org/2/library/profile.html).

[pyprof2calltree](https://pypi.python.org/pypi/pyprof2calltree/), which is a converter for cProfile 
to the kcachegrind graphical calltree analyser, to see nice and colourful schematics of our function calls.

[Kcachegrind](http://kcachegrind.sourceforge.net/html/Home.html), as I just said, is a tool for graphic 
analisys (but not only graphic) of your code calls. There is also a windows port for it.

[line-profiler](http://pythonhosted.org/line_profiler/) is the tool to use after you have some idea of
where the bottlenecks are. It can tell, line by line, where the most of the time is being spent. This is really
awesome and have halped me a lot.


### How to

DISCLAIMER: Remember this is just some pointers to what you can do to profile your code. You should
always refer to the official documentation for more details.

#### cProfile

Take the code you want to profile and run with:

    # Call cProfile
    python -m cProfile [-o output_file] [-s sort_order] myscript.py
    
    # Turn the profile into a callgraphtree
    pyprof2calltree [-i input_file] [-o output_file]


This is the very simple function we are testing.
    
    1 #!/bin/python
    2
    3 z = 0
    4 for j in xrange(5000):
    5     for i in xrange(5000):
    6         z += 1


Profiling with cProfile:

    time python -m cProfile myscript.py
             2 function calls in 2.381 seconds
    
       ncalls  tottime  percall  cumtime  percall filename:lineno(function)
            1    2.381    2.381    2.381    2.381 matriz.py:3(<module>)
            1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
    
    real    0m2.412s
    user    0m2.384s
    sys     0m0.016s
    
Running it without profiling gives us almost the same time.

    time python test.py
    
    real    0m2.363s
    user    0m2.340s
    sys     0m0.012s
    

#### line_profiler and kernprof



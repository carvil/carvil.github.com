---
layout: post
title: "Optimizing the ackermann function using haskell"
category: Programming
tags: ["hashell", "project euler"]
---
{% include JB/setup %}

This article aims to be the first one of a series of articles concerning the
optimization of the [Ackermann function](http://en.wikipedia.org/wiki/Ackermann_function) using Haskell. The problem with this function is that it grows really fast
even with very low numbers. It is not a primitive recursive function, however
it is a total recursive function. Here is the simplest Haskell definition of
it:
   
{% highlight haskell %}
    module Ackermann (ackermann) where
    
    ackermann m n | m == 0 = n+1
                  | m > 0 && n == 0 = ackermann (m-1) 1
                  | m > 0 && n > 0 = ackermann (m-1) (ackermann m (n-1))
{% endhighlight %}

This definition resembles very much its mathematical definition. If one tries
to execute it with low inputs (less than m=4) it runs well, however if one
tries to run this function with m=4 and n=1... well, I never waited long
enough to see its output :-) The biggest problem is the recursion. For
example, if both _n_ and _m_ are greater than zero, the function has a double
recursion, which is very consuming both in terms of time and resources. There
are of course many possibilities for optimizing this function: using fix
point, memoization (by using a state monad) or non-recursively by means of the
[Knuth's up-arrow notation](http://en.wikipedia.org/wiki/Knuth's_up-arrow_notation). 
In further articles I will try each one of those
optimizations and see if I can compute the Ackermann function with m=6 and n=6
:-)


---
layout: post
title: "Maximum digital sum of a googol"
category: Programming
tags: ["hashell", "project_euler"]
---
{% include JB/setup %}

In order to solve
[this](http://projecteuler.net/index.php?section=problems&id=56) problem, I
just created a brute force algorithm in Haskell:

{% highlight haskell %}
    import List
    import Char

    result = maximum [g (a^b) | a <- [1..99], b <- [1..99]]
      where g = sum . map digitToInt . show

    main = print $ result
{% endhighlight %}

The execution is pretty fast:

real 0m0.059s
user 0m0.055s
sys 0m0.003s


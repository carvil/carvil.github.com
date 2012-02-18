---
layout: post
title: "Generate file with n random integers using bagh"
category: Programming
tags: ["bash"]
---
{% include JB/setup %}

Here is an useful bash script that generates a number of random integers into
a file. I use it every time I need to test or to measure the execution time of
a function or an algorithm over integers (for example sorting algorithms):

{% highlight bash %} 
    #!/bin/bash
    
    #declare the random generation function
    fun="od -vAn -N4 -tu4 < /dev/urandom"
    
    #generate a number of integers
    for i in {1..100}; do eval $fun >> temp.txt; done
    
    #delete the empty lines
    sed '/^$/d' temp.txt > temp1.txt
    
    #delete the leading and trailing spaces
    sed 's/^[ \t]*//;s/[ \t]*$//' temp1.txt > numbers.txt
    
    #remove the temporaty files
    rm -rf temp.txt temp1.txt
{% endhighlight %}

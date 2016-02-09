---
layout: post
title: "Problem 22"
category: Programming
tags: ["haskell", "project_euler"]
---
{% include JB/setup %}

I started solving [problem 22](http://projecteuler.net/index.php?section=problems&id=22) by means of the
monad IO. Since Haskell has already functions that read the file into a String
and instantiate the String into a given type (belonging to the type class
Read), it was rather straight forward. However, I made a small change to the
names.txt, adding the list syntax to the list of files, i.e. I added the list
symbols to the file. As one can see from the main function below, I start by
reading the file, then I instantiated the string read into a list of strings
and finally I applied a number of functions to that list of strings.

{% highlight haskell %}
    import List

    main = do x <- readFile "names.txt"
              y <- readNames x
              print ((foldr (+) 0 . values 1 . sort) y)

    readNames :: String -> IO [String]
    readNames = readIO
{% endhighlight %}

The sort function is a pre-defined function in the List library that sorts
the list of strings. Furthermore, the values function calculates the value
of each name, by summing the value of each letter and multiplying it by the
position of the name in the list. Note that instead of looking for the
position each time, an integer is passed as parameter and it is incremented in
order to show the position. Finally, it is just a matter of using a foldr to
sum the list of integers. Below one can find the rest of the functions:

{% highlight haskell %}
    values :: Int -> [String] -> [Int]
    values _ [] = []
    values n (x:xs) = (n * (value x)) : values (n+1) xs

    value = foldr (+) 0 . map getPosition

    getPosition x = case (elemIndex x ['A','B'..'Z']) of
                     Nothing -> 0
                     Just n -> n+1
{% endhighlight %}

The result of GHCi was (0.15 secs, 94072224 bytes)


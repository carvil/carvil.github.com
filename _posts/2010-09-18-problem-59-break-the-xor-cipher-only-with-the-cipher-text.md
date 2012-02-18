---
layout: post
title: "Break the XOR cipher only with the cipher text"
category: Programming
tags: ["hashell", "project euler", "cryptography"]
---
{% include JB/setup %}

[This](http://projecteuler.net/index.php?section=problems&id=59) problem is
about breaking the [XOR cipher](http://en.wikipedia.org/wiki/XOR_cipher) using
only the cipher text and having the following information:

  * The key used to encrypt the original text has length 3;
  * The key contains lower case characters.

In such a simple encryption algorithm, it is actually possible to find the
original text and the key by brute-force all combinations, however I decided
to try something else. It is possible to find the key by doing the following:

  1. First divide the cypher characters into three groups depending on which key character they were encoded from;
  2. Create a distribution of the elements of each group;
  3. Take the first element of each distribution;
  4. XOR those elements with the most used English character (the space, according to wikipedia).

Imagining that the key is "KEY", then for a given plain text, the key repeats
itself until the end. So, for a given plain text INPUTTEXT, the character "K"
of the key encodes the characters "I", "U" and "E" of the plain text; the
character "E" of the key encodes the characters "N", "T" and "X" and so forth.
That is the reason behind the 1st step. The 2nd step aims to verify which
characters are used the most. The distribution will show that and then we just
need to select the most used character of each of the three groups, as
described in the step 3. Finally, in the 4th step we XOR each character from
the step 3 with the space (ascii code 32) and we obtain the key. Then it is
just a matter of decoding the text using that key. Translating this process
into Haskell, I wrote the following function to perform the first 2 steps
mentioned above:
   
{% highlight haskell %}
    isolate l =
      let size = length l
          tmp  = f (size-1)
          l1   = [ (!!) l (i-1) | i <- [ 3*j+1 | j <- [0..tmp] ]]
          l2   = [ (!!) l (i-1) | i <- [ 3*j-1 | j <- [1..tmp] ]]
          l3   = [ (!!) l (i-1) | i <- [ 3*j   | j <- [1..tmp] ]]
      in (select1 l1, select1 l2,select1 l3)
     where f :: Int -> Int
           f n = fromInteger $ round $ fromIntegral n/3
{% endhighlight %}

This function returns the three sorted distributions (select1 comes from
Numeric.Probability.Example.Collection). Then, I extracted the most used
character of the three groups and I obtained the following:

{% highlight haskell %}
    (71,0.174563591022444)
    (79,0.21250000000000013)
    (68,0.19250000000000012)
{% endhighlight %}

So, these three characters (71, 79 and 68) are the most used characters, in
order. Since the most used character in the English language is the space
(ascii 32), it is a matter of XORing the three characters with space to obtain
the key:

{% highlight haskell %}
    xor_int :: Int -> Int -> Int
    xor_int a b = a `xor` b
    
    get_key l = map (chr . xor_int 32) l
    
    *Main> get_key [71,79,68]
    "god"
{% endhighlight %}

Since I have the key now, it is a matter of decoding the whole cipher text and
obtain the plain text. For that, I used the following function:

{% highlight haskell %}
    main = do y <- readFile "cipher1.txt"
              l <- parseList y
              let k = decode [103,111,100] l
              print k
      where
        parseList :: String -> IO [Int]
        parseList = readIO
    
    decode key text =
       let l = take (length text) (cycle key)
           k = xor_list text l
       in map chr k
      where
        xor_list [] [] = []
        xor_list (x:xs) (y:ys) = (x `xor_int` y) : xor_list xs ys
{% endhighlight %}

And the result is: "(The Gospel of John, chapter 1) 1 In the beginning the
Word already existed. He was with God, and he was God. 2 He was in the
beginning with God. 3 He created everything there is. Nothing exists that he
didn't make. 4 Life itself was in him, and this life gives light to everyone.
5 The light shines through the darkness, and the darkness can never extinguish
it. 6 God sent John the Baptist 7 to tell everyone about the light so that
everyone might believe because of his testimony. 8 John himself was not the
light; he was only a witness to the light. 9 The one who is the true light,
who gives light to everyone, was going to come into the world. 10 But although
the world was made through him, the world didn't recognize him when he came.
11 Even in his own land and among his own people, he was not accepted. 12 But
to all who believed him and accepted him, he gave the right to become children
of God. 13 They are reborn! This is not a physical birth resulting from human
passion or plan, this rebirth comes from God.14 So the Word became human and
lived here on earth among us. He was full of unfailing love and faithfulness.
And we have seen his glory, the glory of the only Son of the Father."


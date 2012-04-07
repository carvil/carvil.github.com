---
layout: post
title: "Automatic page classification using artificial neural networks (classifyit.herokuapp.com)"
category: Programming
tags: ["ruby","sinatra","neural networks","classification"]
---
{% include JB/setup %}

Being able to **categorise** is a very human caracteristic. We tend to look at people, objects, ideas, opinions, reactions, etc and immediatly put them in what we think is their right bucket. Person A is introvert, idea B is outrageous, webpage C is technical, etc. 

Why we do it is not the subject I am interested in, though. However, **how** we do it is a very interesting point.

One key way of learning, in my opinion, is learning by example. When we are presented with a new subject, we tend to think of examples that would fit in that specific context. Only after a reasonable number of examples, we are good at understanding a given topic, or recognising patterns.

That is why we, for example, look at a webpage and immediatly know what is the main topic of the page. It's a scientific page, or it is an e-commerce page, or it is a technical blog page. We have seen a lot of different pages and therefore our filters are very accurate.

### What about computers?

Could computers learn as well by example and perform classification? Can we trust computers to perform decent classification?

Well, to a certain extent, I believe so. Therefore, I would like to explore the idea of automatic classification of webpages based in algorithms that learn by example.

One example of such an algorithm is the [backpropagation](http://en.wikipedia.org/wiki/Backpropagation) algorithm, which trains artificial neural networks ([ANNs](http://en.wikipedia.org/wiki/Artificial_neural_network)) in order to infer a function from observations. That function is then used to classify input data.

The ANNs ability to learn by example is very useful when the output is rather subjective or not very well defined. If we provide good example data, the resulting classification function will provide reasonably good results.

### Idea

My intention is to create a simple program that, given a url of a webpage, will respond to this simple question: 

*Is this a tech site?*

I would like to know, given a URL, if the page is about technology or not. 

### Implementation

In order to do that, I followed a very simple algorithm described below. First, there is some pre-processing needed:

- I started by defining what I considered to be a basic set of [words](TODO) that are usually present in technology pages (disclaimer: please note that this is just a very basic example and therefore doesn't aim at accurately representing what is the content of a technical site);
- I also defined a couple of datasets that could be representative of a technical page in terms of word counts based on the set of words defined in step 1; and a dataset representative of a webpage that is not technical, again using word counts.

After defining the datasets described above, the sinatra application will, on startup, do the following:

- Train an ANN using the ruby gem [ai4r](http://ai4r.rubyforge.org/). To achieve that, I added the datasets defined in step 2 in order to give good positive and negative examples, so that the resulting function can perform classification in an acceptable way;
- Once the network is trained, the application is ready to receive input URLs.

Upon request, the application will:

1. Fetch the entire page content;
2. Count the frequency of the words in the page content;
3. Discard the words that are not a part of the dataset defined in the very first point mentioned above;
4. Run the classification function resulting from training mentioned above and output the result.

Hopefully, if the assumptions are correct, one will get reasonable results. I tried with a few websites and, in general, the algorithm is capable of classifying technical webpages, with a few exceptions.

### What not to do

Along the way, I experienced some problems. For example, one has to be careful with the words one chooses to use. Initially, I was considering words such as *technology*, *iphone* and *method*, which are present in many websites, regardless of the nature of the websites. For example, news websites such as bbc.co.uk and guardian.co.uk were false positives when using such words. 

It is also important to havegood test data, otherwise the results won't be accurate (or will be accurate in relation to the testing data, but meaningless towards what we are trying to classify).

Finally, it is also very important to have a good decision function. The ANN classification will return two results: the probability of the page being technical and the probability of the page not being technical. In my case, I consider a site to be technical if the probability of being technical is greater than the probability of not being technical, or if the probability of being technical is greater than 90%.

Minor changes in both the training data and the decision function will have a big impact in the results, so it is very important to get those 2 factors right.

### Conclusion

Even with very lightweight examples and classification rules, the algorithm seems to classify websites with reasonable accuracy. 

I tested with sites such as wired and ycombinator; blogs such as github, heroku and daring fireball. In terms of negative tests, I tried news sites such as bbc and guardian, among others.

My gut feeling is that with proper training data and better keywords, one could expect much better results and therefore achieve a better classification score.

Working with ANNs was a good experience and I will keep them in mind for future classification problems.


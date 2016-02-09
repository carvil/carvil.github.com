---
layout: post
title: "Determining the most efficient way to connect a network"
category: Programming
tags: ["java", "project_euler"]
---
{% include JB/setup %}

The [107th](http://projecteuler.net/index.php?section=problems&id=107) Euler
Problem aims to calculate the most efficient way to connect a given network.
This is definitely a problem that can be solved using graphs, i.e. it's a
typical minimum spanning tree (MST) problem (Prim's algorithm). Please note
that the code below is just a snapshot of the original code. If one wants to
check the entire code, please go to my
[github](http://github.com/carvil/projecteuler) repository. I started by
creating a class to represent the graph. My representation uses an adjacency
matrix, however for space-saving purpose I could have used adjacency lists
instead.

{% highlight java %}
    public class Graph {

    	public int nvertices;
    	public int[][] edges;

            ...
{% endhighlight %}

Then I created a Prim's class which has a method to calculate the minimum
spanning tree of a given graph. The method, named prim, is explained below
together with the class:

{% highlight java %}
    public class Prim {

      public Graph g;
      public int start;

      private final int MAXINT = Integer.MAX_VALUE;
      private int[] distance;
      private int[] parent;
{% endhighlight %}

As one can see, there are a number of global variables: the graph, the
starting node, the maximum integer value (explained below) and two arrays -
one containing the distances and another containing the parents. These are
very important variables for the prim's method below:

{% highlight java %}
    public int prim() {
      //Variables for loops and saving information
      int i, v, weight, dist;
      //Total amount saved by creating a MST
      int save = 0;
      //Number of vertices
      int nVertices = this.g.getNvertices();
      //Current elements in the final tree
      boolean[] intree = new boolean[nVertices];

      //Initialization of the arrays
      for (i=0; i < nVertices; i++) {
        intree[i] = false;
        this.distance[i] = this.MAXINT;
        this.parent[i] = -1;
      }
{% endhighlight %}

These first lines initialize the helper variables and the arrays which will
hold the information regarding the MST. Furthermore, the following code is the
responsible for creating the MST and calculate the total saved:

{% highlight java %}
      //Starting point
      this.distance[this.start] = 0;
      v = this.start;

      //Loop through the vertices
      while(intree[v] == false) {
        intree[v] = true;

        // Search all adjacent nodes and update distances
        for(int j=0; j<nVertices; j++) {
          weight = this.g.edges[v][j];
          if (weight != 0) {
    	if((this.distance[j] > weight) &&
    	   intree[j] == false) {
    	  if (this.distance[j] != this.MAXINT)
    	    save += this.distance[j];
    	  this.distance[j] = weight;
    	  this.parent[j] = v;
    	}
    	else if (intree[j] == false)
    	       save += weight;
          }
        }

        //Find out next vertice to explore
        v = 0;
        dist = this.MAXINT;
        for(i=0; i< nVertices; i++) {
    	if ((intree[i] == false) &&
    	    (dist > this.distance[i])) {
    	  dist = this.distance[i];
    	  v = i;
    	}
        }
      }

      //Return the total saved
      return save;
{% endhighlight %}

For the scope of this problem, the _save_ variable is important since it is
accumulating the total savings during the process of building the MST. Then it
was a matter of parsing the input file into a matrix and pass it on to the
Prim's class in order to perform the calculations. The Main class is also
available in my [github](http://github.com/carvil/projecteuler) repository.


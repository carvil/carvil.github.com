---
layout: post
title: "QuickSort - Haskell vs C"
category: Programming
tags: ["hashell", "C", "project_euler"]
---
{% include JB/setup %}

I was playing around with some sorting algorithms and I decided to try to
compare the implementation of the QuickSort algorithm in Haskell and in C.

  * C in-place partition implementation
Starting by C, I implemented an in-place partition algorithm. This means that
no auxiliary memory is used by the program besides the memory needed to store
the array. Here is the definition of QuickSort:

{% highlight c %}
    void quicksort(long unsigned int *array, long int min, long int max) {

      long int pivotIndex;

      if (max > min) {

        pivotIndex = partition(array,min,max);
        quicksort(array,min,pivotIndex-1);
        quicksort(array,pivotIndex+1,max);
      }

    }
{% endhighlight %}

The basic idea is to select a pivot and move all the numbers smaller than the
pivot to the left side of the array and the bigger numbers to the right side
of the array. Then, proceed with the same algorithm for both sub-lists. The
partition procedure swaps the elements of the array around according to the
pivot:

{% highlight c %}
    long int partition(long unsigned int *array, long int min, long int max) {

      long int left = min,
               right = max,
               pivotValue = array[min],
               pivot = min;

      while ( left < right ) {

        while( array[left] <= pivotValue ) left++;
        while( array[right] > pivotValue ) right--;

        if ( left < right ) swap(array,left,right);
      }

      array[min] = array[right];
      array[right] = pivotValue;

      return right;
    }
{% endhighlight %}

Finally, a swap procedure:

{% highlight c %}
    void swap(long unsigned int *array,long int from, long int to) {

      long unsigned int temp = array[from];
      array[from] = array[to];
      array[to] = temp;
    }
{% endhighlight %}

As one can see, it is a lot of code just to implement the QuickSort algorithm.
The execution time is of course related to the choice of a good pivot. In
order to test it and check its performance, I have generated a file with 10.000 random integers using this
[script](http://vilhena.wordpress.com/2010/07/03/bash-generate-file-with-n-random-integers/).
I also include the number of integers in the top of the
script to make it easier to allocate memory :-) Then I read this file into an
array, sort it and write it back to a file. Here is the main function (please
note that no error handling was made in order to keep the code easier to read
for its purpose):

{% highlight c %}
    int main() {

      long int size = 0, min = 0;
      long unsigned int *array;
      long int i, j = 0;
      FILE *fpin = fopen("numbers.txt","r");
      FILE *fpout = fopen("sorted_numbers.txt","w");
      long unsigned int line = 0;
      char buffer[256];

      //Step 0 - Read first line and get the size of the file
      if(feof(fpin) == 0) {

        fscanf(fpin,"%d\n",&size);
      }

      array = (long unsigned int*) malloc(size*sizeof(long unsigned int));

      //Step 1 - read input file into array
      while( fgets(buffer,256,fpin) != NULL ) {

        array[j++] = strtoul(buffer,NULL,0);
      }

      fclose(fpin);

      //Step 2 - sort array
      quicksort(array,min,size-1);

      //Step 3 - write array into output file
      for(i=0; i < size; i++) {

        fprintf(fpout,"%lu\n",array[i]);
      }

      fclose(fpout);

      return 0;

    }
{% endhighlight %}

Here are the results:

Carlos-Vilhenas-iMac:C vilhena$ time ./Sort

real 0m0.006s
user 0m0.004s
sys 0m0.002s

This is really fast! However, it didn't work in the first try. I had to change
the stack size in order to run it. For that purpose, I used the command
_ulimit -S -s unlimited_.

  * Haskell implementation
It is shocking how compact this definition can be in Haskell. Here is my
implementation:

{% highlight haskell %}
    qsort :: Ord a => [a] -> [a]
    qsort [] = []
    qsort (x:xs) =
      let smaller = filter (<x) xs
          bigger  = filter (>= x) xs
      in qsort smaller ++ [x] ++ qsort bigger
{% endhighlight %}

This Haskell implementation is around 85% smaller than the C version!! It was
much easier to test and I found out almost no errors compared to the C
version. Here is a main function in order to run it:

{% highlight haskell %}
    main = do y <- readFile "numbers.txt"
              l <- parseList y
              let s = qsort l
              writeFile "sorted_numbers.txt" (show s)
              print "done."
      where
        parseList :: String -> IO [Integer]
        parseList = readIO
{% endhighlight %}

I compiled this with the command: `ghc -O3 -o Sort Sort.hs` and I executed it
with: `time ./Sort +RTS -H64k -k64k`. Even though the definition is much smaller
than the C version, the execution time was not faster: `real 0m0.099s user
0m0.092s sys 0m0.006s`. I found it quite inefficient when compared to the C
version, so I compiled it with profiling information: `ghc -prof -auto-all -O3
-o Sort Sort.hs.`

  * In conclusion, although the Haskell version is much cleaner and faster to develop/test, it is though slower than the C version. I also experimented with 90.000 integers and the time difference is much more evident, with the C version achieving 0m0.035s while the Haskell version achieved 0m0.635s of real execution time.


---
layout: post
title:  "Passing functions around in C"
date:   2017-1-4 15:01:00
categories: clang function
image: /assets/article_images/2017-1-4/functions.jpg
image2: /assets/article_images/2017-1-4/functions-mobile.jpg
---

It has been sometime since I have used this, but back when I was taking `C for C++ Programmers` course in my undergraduate days, I did not know that you can pass function into a C function similar to passing functions in Javascript of other languages like Swift.

Below I am going to show how it is done using the `void` type pointer. For this example I am going to run bunch of sort functions on an array of integers and compare the time it takes to run them. To make it easy to maintain and add different sorts in the future I decided to make an array of functions in C over which I loop through and call the function and pass the array of integers into it.

{% highlight c %}
int main( )
{  
  int orig[ MAX ], copy[ MAX ], i;
  
  /* array that hold sorts that will be performed in sequential order */
  void *function[ NUM_SORT ] = { &selection_sort, &quick_sort, &shell_sort, 
                                 &stdlib_qsort, &insertion_sort, &bubble_sort };
  /* array that holds sort names respective to the sorts that will be 
     performed */
  char *sort_name[ NUM_SORT ] = { "Selection sort", "Quicksort", "Shellsort", 
                                  "Qsort", "Insertion sort", "Bubble sort" };
  
  fill_array( orig, MAX );
  
  /* sorts being executed */
  for( i = 0; i < NUM_SORT; i++ ) {
    execute_sort( orig, copy, MAX, sort_name[ i ], function[ i ] ); 
  }
  printf( "\n" );
  
  return (EXIT_SUCCESS);
  
}
{% endhighlight %}

In the code above I created an array of functions using `void *function[ NUM_SORT ]` where the array holds `void*` pointers that point to the address of the function. In C you can get the address of the function by appending `&` in front of it as such `&selection_sort`.

Although it is now shown but above the functions are defined including a function called `execute_sort` function. This takes in 5 arguments. The last one is the important one which contains the function pointer being passed. Lets see how we define the parameter signature of the `execute_sort` function to take a pointer to another function and how to call that function.

{% highlight c %}
void execute_sort( int const orig[], int copy[], int array_size, 
                   char* sort_name, void (*sort_function)(int[], int) ) 
{
  
  cpy_array( orig, copy, array_size );//Copy original random array
  
  clock_t before = clock(); //start timer
  
  sort_function( copy, array_size ); //execute function;
  
  printf( "\n%s of %d items took %f seconds\n", sort_name, array_size, 
          (double)(clock() - before )/CLOCKS_PER_SEC );
  
}
{% endhighlight %}

As you can see we tell a function that it should expect a function as a parameter by using `void (*sort_function)(int[], int)` signature. You need to specify the parameter types of the function being passed into the `execute_sort` function which we did as all of the sort functions take an array of ints are their first argument and the size of the array as int as the second argument.

You can then call the function as such:
{% highlight c %}
sort_function( copy, array_size );
{% endhighlight %}

It is as simple as that. You can view the full source code I used in this example [here](https://github.com/ankurp/C-Algorithms/blob/master/sorting/sorting.c) and also find instructions on how to compile and run the code.

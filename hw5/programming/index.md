---
layout: asides
toc: true
tasks: true
title: Homework 5 Programming
---

# HW5: Programming Assignment

+ Due: Friday, November 19th, 11:59pm PST

+ To access the written portion of this assignment, click [here](..)

+ Directory name in your github repository for this homework (case sensitive): `hw5`

  - There is no skeleton code for this assignment.
  - You **MUST** provide a `Makefile` so that we can compile your code (not run it) by simply typing `make` which should, among other compilation commands, produce an executable for your problem 2 programs.
  - Remember to compile and test your code inside Docker (but should do your git commands outside Docker)
  - Provide a `README.md` file to explain how to compile your code, and to document any oddities you want the graders to be aware of.
  

### Problem 1 (Create a hash table, 40%)

You will create a Hashtable<string,T> data structure with quadratic probing for collision resolution. We will use only strings made of lowercase letters, and no other characters. Your solution to this problem should be in `Hashtable.h`. 

You will need to implement the following functions:

+ `Hashtable (bool debug = false, unsigned int size = 11)`: Constructor for the Hashtable, initially allocating size elements to the array.  The effect of the Boolean `debug` flag is described below.
+ `~Hashtable ()`: Destructor
+ `int add (string k, const T& val)`: If k is already in the Hashtable, then do nothing.  If it is new, add it to the Hashtable with the specified value.  Returns the number of probes required to place k (if no probes are required, return 0).
+ `const T& lookup (string k)`: Returns the value associated with k.  Don't crash if k is not in the Hashtable, but you can return whatever T value you like (possibly something else in the Hashtable, or a garbage value).
+ `void reportAll (ostream &) const` : output, to the given stream, every (key,value) pair, in the order they appear in the Hashtable;  on each line, output the key, followed by a space, followed by the value, and then a newline.  Note that this requires the T type to have operator<< defined.
+ `resize ()`:  A private helper function which approximately doubles the number of indices available.  You call this function when an insert would increase the load factor above 0.5.  The number of indices should follow this sequence: 11, 23, 47, 97, 197, 397, 797, 1597, 3203, 6421, 12853, 25717, 51437, 102877, 205759, 411527, 823117. If the size variable was used in the constructor, your first resize should be to the next largest number in this sequence (so, if the initial size is 195, only increase the table to 197).  After resizing the table, you need to rehash all strings and their values, and you must rehash all of the strings starting from the beginning of the table and proceeding in order to the end of the table.
+ `int hash (string k) const`:  This private/protected function takes a string as input, and outputs a pseudo-random index to store it at. More detail on how to write this hash function is provided below.

When you create a Hashtable or resize a Hashtable without `debug` mode, you should generate 5 random numbers between 0 and m-1 (inclusive): we'll refer to them as r1, r2, r3, r4, r5.  These change every time you resize the Hashtable.  When you create a debug-mode Hashtable, you must always set r1=983132572, r2=1468777056, r3=552714139, r4=984953261, r5=261934300 (these numbers were generated by `random.org`).

You will use the above 5 integers in your hash function detailed below.

#### Writing a Hash Function

First translate each letter into a value between 1 and 26, where a=1, b=2, c=3, ..., z=26.
You can translate a string of 6 letters a1 a2 a3 a4 a5 a6 into an int via the following mathematical formula:

`27^5 a1 + 27^4 a2 + 27^3 a3 + 27^2 a4 + 27 a5 + a6`

Place zeros in the leading positions if your word is shorter than 6 letters.  So `abc` would set a1, a2, and a3 all to zero.

If an input word is longer than 6 letters, then you should first do the above process for the last 6 letters in the word, then repeat the process for each successive 6 letters.  You will never receive a word longer than "antidisestablishmentarianism" (that is, 28 letters), which should result in a sequence of no more than 5 ints `w1 w2 w3 w4 w5`, where `w5` was produced by the last 6 letters of the word.

Store these values in an int array. Place zeros in the leading positions if the word was shorter than 25 letters.  So for a string of 12 letters, w1, w2, and w3 would all be 0.

We will now hash the word. Use the following formula to produce the result, and make sure to cast this into a `long long` at the appropriate places so that you don't have an overflow error: the compiler will assume each intermediate value is an int if you don't specify this.  `m` is the number of indices in the Hashtable:

`(r1 w1 + r2 w2 + r3 w3 + r4 w4 + r5 w5) % m`

### Problem 2 (Collect Data, 10%)

Does your Hashtable live up to its hype?  Now you can find out!  Using variable initial Hashtable sizes, the return value for your add function, and whatever test cases you want to run on, collect data you are interested in, and report on your results in your README file.  Unsure what to do?  Here are some suggestions:

1. **Confirm the Birthday Paradox:**  With an initial size of 365, add items until the first probe occurs or (very unlikely) you resize the array.  Run this a bunch of times (at least 1000).  You should find that approximately 50.7% of the time you insert 23 things or less, and the rest of the time you insert 24 things or more.
2. **Probe Rate:** How many probes occur, on average, when inserting n elements into the Hashtable?  When the Hashtable is (almost) half full, you should expect 1 probe on average.  When the Hashtable is about a quarter full, you should expect 1/3 probes on average.  Since your Hashtable oscillates between these loading factors when resizing, you should probably find something a bit smaller than 2n/3.
3. **Ruin Quadratic Probing:** Start with an intial Hashtable size that is non-prime, and find a sequence of inputs that causes your program to hang (or trigger a failsafe if you programmed one in).  It may be easier to randomly generate a lot of test cases and run them until something fails than to systematically try and find a test case by hand.  You should be able to do this for a Hashtable size of 12 or larger.

Any two of the above ideas, or similar ideas of your own, are sufficient for this part of the assignment.

### Finishing Up

### Completion Checklist

+ Directory name for this homework (case sensitive): `hw5`
  - This directory should be in your `hw-username` repository
  - This directory needs its own `README.md` file briefly describing your work
  - `Hashtable.h`, and any `.cpp` files created for problem 2.
  - Your `Makefile`
+ The submission link will be posted on Piazza a few days before the due date.


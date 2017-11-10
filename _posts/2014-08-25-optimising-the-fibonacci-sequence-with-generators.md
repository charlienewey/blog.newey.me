---
layout: post
title: Optimising the Fibonacci Sequence with Generators
date: 2014-08-25 21:17:28.000000000 +01:00
---

So recently, a friend of mine was talking about ways of calculating the sum of
the first *n* numbers in the Fibonacci sequence, and it gave me an idea.

<!-- more -->

The Fibonacci sequence is very well known, and it's an interesting construct -
both mathematically and computationally. As well as commonly being taught in
mathematics classes as a simple introduction to sequences, the Fibonacci
sequence is often used as a method for introducing beginner programmers to the
principle of recursion.

Most progammers will know the principle of recursion well and will probably be
able to write a recursive function to generate the Fibonacci sequence already -
but for those who are unfamiliar with it, the next section may be useful.

#### You don't understand recursion until you understand recursion

Generally speaking, a naive recursive function to generate the nth term in the
Fibonacci sequence will look something like this:

```language-python
def calc_fibonacci(n):
    """Generate the nth Fibonacci number"""
        if n == 0:
                return 0
    if n == 1:
        return 1
    else:
        return (calc_fibonacci(n - 1) + calc_fibonacci(n - 2))
```

The function defined above is *recursive* because it is *self-defining* - that
is, it calls itself over and over until it reaches what's known as the *base
case* - the point at which the function can't call itself any more. It's
sensible, mathematically elegant, and easy to understand - so what exactly is
the problem with it?

Well, *recursion* as a concept is sensible and logical; in fact, it complements
the recurrent sequence constructs that are commonly used in mathematics. The
problem comes when writing programming code in such a way - a computer has to
keep track of how many times a recursive method has called itself - or the
"depth" to which it has recursed, and this can cause a significant overhead if
a method calls itself lots of times (calculating the 30th Fibonacci number
using the method above (```calc_fib(30)```) results in over 2.5 million
function calls).

Essentially what happens when running recursive code is that the machine will
enter the initial function cal and it will progress through the function,
executing each line of code until it reaches the point at which the function
calls itself. As the function calls itself, the computer will mark the current
place in the execution stack and execute the next recursive function call. This
process continues, with the computer following each recursive call down through
the stack, until execution reaches the base case (i.e. the point at which the
function stops recursion - in this case, ```n == 0``` or ```n == 1```). Once
this happens, values start being returned and execution starts coming back
**up** through the stack (collecting all of the terms along the way) until
execution arrives back at the original function call, and the *nth* term in the
Fibonacci sequence is returned as an integer, as if by magic.

If the paragraph above doesn't make much sense, watch [this excellent video on
recursion by Computerphile](http://www.youtube.com/watch?v=Mv9NEXX1VHc) - it
will really help! Actually, while you're at it, you could watch [this video on
recursively generating the Fibonacci
sequence](http://www.youtube.com/watch?v=7t_pTlH9HwA) too.

So, moving on - to generate the Fibonacci sequence as a list, perhaps one could
use something like this:

```language-python
def fib_seq_recursive(n):
        """Generate Fibonacci sequence from 1 to n"""
    seq = []
    for i in xrange(1, n):
        seq.append(calc_fibonacci(i))
    return seq
```

For something like calculating the sum of the Fibonacci sequence, it's pretty
inefficient keeping track of such a large recursion stack, and the sheer number
of recursive calls make the above method very slow to run - so how do we go
about generating the Fibonacci sequence efficiently?

#### Whose loops? My loops?!

Most programming languages are useful in that they come with pre-rolled tools
for optimising recursion -- *loops*. I was being facetious with that last
sentence, but it's true. Compared to recursion, the overhead of executing a
logical loop is minimal. It's not necessary to keep track of recursive function
calls, and an operation can be applied to a set of terms *iteratively* - which
helps for generating sequences where the output depends on the previous element
in the sequence.

For example, it's relatively fast (and very easy) to generate the Fibonacci
sequence iteratively:

```language-python
def fib_seq_list(n):
    """Generate Fibonacci sequence from 1 to n"""
    previous = 0
    current = 1

    sequence = [previous, current]
    for i in xrange(1, n):
        tmp = previous
        previous = current
        current += tmp

        sequence.append(current)

    return sequence
```

Something like this is excellent for calculating the sum of the first *n*
digits of a sequence, as you can simply use the Python built-in `sum()` to do
the legwork for you. For example, the sum of the first 20 digits of the
Fibonacci sequence can be calculated with:

```language-python
sum(gen_fib_seq(20))
```

#### Making your CPU cycles go further

The iterative solution is much better than recursion, but we're still creating
a list structure and holding all of those integers in memory. Imagine a
hypothetical scenario where you had to generate the first million integers in
such a sequence - obviously as the numbers get progressively larger, they
occupy progressively larger chunk of memory. Imagine holding several million
arbitrarily-sized integers in memory all at once - it'd be a memory-hogging
monstrosity - as well as the (slight) CPU overhead of creating a new
linked-list node for each sequence item.

Luckily, Python has a useful language feature that helps with such a task -
generators. For a simple, accessible explanation of what Python's generators
are, how they work, and what they're capable of, check out [this excellent blog
post by Jeff
Knupp](http://www.jeffknupp.com/blog/2013/04/07/improve-your-python-yield-and-generators-explained/).
I'll summarise the concept here, but for a more in-depth explanatio you really
should check that blog post out. Generators are slightly different to
traditional methods/functions. Instead of being used as *subroutines* and
running independently of the main function, Python generators are *coroutines*
- which essentially means that state is retained between iterations. For
example, if a generator is used in a loop, the generator's state will be kept
between iterations and it will `yield` a single integer on each iteration, but
without losing it's place in the sequence.

This generator function generates the Fibonacci sequence, but without the
overhead of a list:

```language-python
def fib_seq_gen(n):
        """Generate Fibonacci sequence from 1 to n"""
    a = 0
    b = 1

    yield b
    for i in xrange(1, n):
        tmp = a
        a = b
        b += tmp
        yield b
```

The proof of the pudding is in the eating, so to speak, so I knocked [this
script](https://gist.github.com/charlienewey/da4616e9130f5a9d4682) together to
test various performance aspects of each of the methods I've detailed above.
The results are as follows:

```
--- Average Memory Consumption (MB) ---
Recursive:  12.4217722039
List:  12.4518229167
Generator:  12.453125

--- Maximum Memory Consumption (MB) ---
Recursive:  12.421875
List:  12.453125
Generator:  12.453125

--- Average Execution Time (seconds) ---
Recursive:  37.107519865
List:  3.31401824951e-05
Generator:  5.96046447754e-06
```

Interestingly enough, the results were not what I expected! It appears that
recursion is quite memory-efficient inside Python, even though it seems
counter-intuitive - the recursive function appears to use the least memory of
all three methods, while the generator function appears to use the most.

However, when looking at the timing benchmarks, the benefits of using an
iterative method become more obvious. The recursive function took approximately
37 seconds to generate the first 35 Fibonacci numbers... while the other two
used only a few thousandths of a second. The generator function is the fastest
of all - running in approximately 1/5th of the time of the list function.

From this point, it becomes clear that the recursive function is so slow as to
be almost useless, but the difference between the list function and the
generator function appears to be quite small for a small sequence - so what
happens when generating very large sequences using the list and generator
functions? Using the two iterative functions to generate the first *50,000*
numbers in the Fibonacci sequence, the performance boost from using a generator
became **much** more obvious.

```
--- Average Memory Consumption (MB) ---
List:  196.295973558
Generator:  31.62890625

--- Maximum Memory Consumption (MB) ---
List:  238.21875
Generator:  31.62890625

--- Average Execution Time (seconds) ---
List:  0.0932259559631
Generator:  5.00679016113e-06
```

Looking at the results, the generator function uses over 150 megabytes less
memory, and runs approximately 18,000 times faster than the list function!

So there you have it - generators are nutritious and good for programmers. You
should use them when you can as part of your 5-a-day. (have I taken this
vegatable metaphor too far?)

#### Summary
* Recursion is an extremely powerful programming tool, but it can be really
  slow if used badly.
* Generators (aka coroutines) are nifty things for large sequences - they can
  save a **great deal** of memory and CPU time when used properly

#### Resources
* [Computerphile video on Recursion](http://www.youtube.com/watch?v=Mv9NEXX1VHc)
* [Computerphile video on Recursive Fibonacci](http://www.youtube.com/watch?v=7t_pTlH9HwA)
* [Jeff Knupp's blog post on Python generators](http://www.jeffknupp.com/blog/2013/04/07/improve-your-python-yield-and-generators-explained/)

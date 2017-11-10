---
layout: post
title: The Vietnam Snake Puzzle
date: 2015-05-21 11:23:00.000000000 +01:00
---

I spotted a post on the Guardian this morning that posed an interesting
problem. It's called the [Vietnam Snake][1], and it's apparently a problem
that's been given to Vietnamese primary-school children as part of their
mathematics class. The idea is that you follow the "snake" and you fill in the
numbers 1 to 9 in an order that satisfies the equation.

<!-- more -->

I've borrowed the original image from the Guardian/VN Express here just to show
the puzzle: ![The Vietnam Snake
Puzzle](/images/vietnam-snake.png)

It's a bit of a cruel problem to give to 8-year-olds, because there isn't
really a way of solving it using logic - or really, any way of solving it
intelligently at all. The only way to solve it is by trial and error, which
involves lots of tedious plugging-in of numbers. I can only imagine how much
fun that class must have been.

It grabbed my attention, though - it's an interesting problem (faintly
reminiscent of something that [Project Euler][2] might have), and I wanted to
see the solution. As it happens, I'm a programmer and I'm *far* too lazy to
plug in all those numbers myself. Script time!

The solution took me about 5 minutes to write, and it was surprisingly
straightforward - you don't need to worry about operator precedence because
there are no parentheses in the puzzle. It's a fairly simple permutation
problem, and the algorithm looks something like this:

* generate all permutations of the sequence 1 to 9
* for each permutation:
    * check if it satisfies the equation
    * if so, print it out

Interestingly, it turns out that there is more than one solution - in fact,
there are 128!

Here's my (Python) solution to the problem:

```python
from __future__ import division
import itertools

def nam(seq):
    return (seq[0] + 13 * seq[1] / seq[2] +
            seq[3] + 12 * seq[4] - seq[5] -
            11 + seq[6] * seq[7] / seq[8] - 10) == 66

perms = itertools.permutations(range(1, 10))
for solution in filter(nam, perms):
    print(solution)
```


[1]: http://www.theguardian.com/science/alexs-adventures-in-numberland/2015/may/20/can-you-do-the-maths-puzzle-for-vietnamese-eight-year-olds-that-has-stumped-parents-and-teachers
[2]: https://projecteuler.net

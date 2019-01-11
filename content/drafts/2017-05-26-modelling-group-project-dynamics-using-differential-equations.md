---
title: Modelling Group Projects with Differential Equations
date: 2017-05-26
meta: true
draft: true
---

Ever done a group project at university or at work? Ever noticed how some people
seem to do all the work, some others can't seem to do even the most basic tasks,
and the rest tend to disappear (never to be seen or heard from again)? Like me,
you're probably one of the former category.

I was set the task of modelling the dynamics of group projects as coursework for
the awesome [Simulation Modelling][1] module at Southampton University, and I've
decided to adapt it into a blog post because I haven't posted for ages - so
let's get cracking.

Firstly, let's come up with some questions to answer.

* How do hard-working students contribute to the success of group projects?
* How do lazy students contribute?
* How do the efforts/rewards vary for hard (and lazy) students?
* Does group size affect the dynamics of the groups?

It turns out, you can model certain aspects of this situation using differential
equations; population dynamics is a pretty common problem to model in fields
like evolutionary game theory, mathematical biology, and loads of others. It's
actually a reasonably well-understood problem, so it should be quite
straightforward to construct a simple model and see what conclusions we can get
from it.

![A relevant comic from "Toothpaste For
Dinner".](/content/images/2017/06/group-assignment-2.gif)

### Defining the problem

So the problem definition that I've given you is a bit woolly at the moment -
let's tighten things up a bit. I'm going to go ahead and make some assumptions
to make constructing a model easier - just so that we can work with continuous
quantities. (There are some issues with this approach that I'll highlight later,
but let's tackle the easy problems first).

* There is an infinite population of students.
* The population is *well-mixed* (that is, groups are completely random).
* Each student takes part in *one* group project per semester.
* At the beginning of each semester, students are assigned *random* groups of
  size **N**.
* At the end of each semester, the *marks* for each group is the *sum* of the
  total group effort.
* Each *individual's* mark is simply the group mark divided by the number of
  group members.
* Each student has an assigned *strategy* - to *work hard* (**H**) or *be lazy*
  (**L**). Hard workers put more effort in.
* The *payoff* for each strategy is going to be each individual's mark *minus*
  the amount of effort that they put in.
* There is a coefficient for the *cost* of effort - basically it controls how
  much effort it costs each individual to put work into a group project.
* At the *end* of each semester, each student reconsiders their strategy (they
  may switch to working *hard*, or being *lazy*). This change will be
  *proportional* to the difference in payoff of some other *randomly-selected*
  student that they will compare themselves to (don't worry if this isn't clear
  now, I'll explain more later).

### Formalising a simple model

Because of the assumptions that we made above (i.e. an infinite and well-mixed
population of students), we can start constructing a mathematical model to
describe the evolution of this situation. For the sake of clarity, let's start
defining a bit of mathematical notation so that this blog post doesn't have to
be a million words long.

* Each group is of of size \\( N \\).
* The number of *hard workers* in each group is \\( h \\), and the number of
  lazy workers is \\( l \\).
* Let's denote the *effort* contributed by hard workers as \\( H \\), and the
  effort from the lazy ones as \\( L \\).
* The *cost* of effort is uniform across the entire population. Let's call it
  \\( a \\).
* The mark for the group is the total effort; \\( e\_{G} = h \cdot H + l \cdot L
  \\).
* The mark for each individual is evenly distributed across group members; \\( m
  = \frac{e\_{G}}{N} \\).

Right, now we've got that out of the way. It behooves us at this stage (I've
wanted to casually slip that word into a conversation for *ages*) to start
thinking about the *average* case instead of thinking in terms of *individual
groups*.  let's go ahead and think about the *average group* over this infinite
population (and thus their *average effort*). To this end, let's change the
definition \\( h \\) and \\( l \\) a bit - let's use them to to denote the
*fraction* of the overall population that are hard workers and lazy workers.

Firstly, we need to think about the average composition of each group. We know
that a fraction \\( h \\) of the population is hard-working, and the size of
each group is \\( N \\). So then, the average number of hard workers in each
group (across our infinite population of ~~monkeys~~ students) is \\( h \cdot N
\\). The same goes for lazy workers - \\( l \cdot N \\). Let's simplify a bit -
we know that students are *either* hard-working or lazy (there is no other
strategy), so we know that \\( h + l = 1 \\), and we can define \\( l = 1 - h
\\) for simplicity.

Now that we know the average composition of each group, it follows quite easily
that the average mark for each is simply the total effort from the average group
composition of hard/lazy workers. That is;

\\[ \bar{e} = (l \\cdot N \\cdot L) + (h \\cdot N \\cdot H) \\] \\[ \bar{e} =
((1 - h) \cdot N \cdot L) + (h \cdot N \cdot H) \\]

Then we know our average mark;

\\[ \bar{m} = \frac{\bar{e}}{N} \\]

Notice that in our definitions of effort and individual mark above, we're
multiplying by \\( N \\) and then dividing by \\( N \\). This should be setting
off your spidey-senses - it looks pretty unlikely that group size will have any
effect on the aggregate strategy. This is something that we might want to test
later.

Now we need to recall that each student's *payoff* is their individual mark
*minus* the effort that they individually put into on the project multiplied by
some cost coefficient \\( a \\). Let's call the respective payoffs for the
hard-working strategy \\( \pi\_{H} \\), and the lazy strategy \\( \pi\_{L} \\).
From the definition given above, it follows that;

\\[ \pi\_{H} = \bar{m} - a \cdot H \\]

\\[ \pi\_{L} = \bar{m} - a \cdot L \\]

And the average payoff across the *whole population*? Simply multiply the
payoffs by the prevalence of each strategy;

\\[ \bar{\pi} = \pi\_{H} \cdot h + \pi\_{L} \cdot l \\]

Right. Finally, let's consider the tricky bit - *strategy switching*. At the end
of each course, each student reconsiders their strategy by picking some other
student at random (note that the randomly-selected student doesn't have to be
from the same group). If the randomly-selected student has a *higher payoff*,
the first student will switch to that strategy with a probability *proportional
to the difference in payoff*. Of course, if their strategies happen to be the
same (i.e two hard workers comparing against each other), then they won't
switch. Obviously, the strategy with a higher payoff will have more students
switching to it. But *how many* more students?

The probability of a hard worker meeting a lazy worker is simply \\( h \cdot
(1-h) \\). Same goes for a lazy worker meeting a hard worker - \\( (1-h) \cdot h
\\). The average difference in payoff between a hard worker and a lazy worker is
\\( \pi\_{H} \pi\_{L} \\). After a little bit of algebra, it turns out that the
probability of *the average* hard-working student switching to a lazy strategy
is simply the difference in hard-working payoff from the population's average
payoff: \\( \pi\_{H} - \bar{\pi} \\).

We're nearly there - now we just need to smoosh everything together into one
differential equation.

The change over time of the proportion of hard-working students (in the average
case) can be thought of as the difference in payoff from the average - weighted
by the fraction of hard workers that are currently in the population. Happily,
this leads to a pretty common form of differential equation known as the
*replicator equation* - a well-known format that has often been used to describe
population dynamics in evolutionary game theory. The final product (phrased in
terms of hard-working students) looks like this;

\\[ \frac{dh}{dt} = h \cdot ( \pi\_{H} - \bar{\pi} ) \\]

So... now what?


### What happens?

First port of call is to plug this model into Python and run a simulation of a
few semesters with a few different parameters, just to see what happens. To
start with, let's say that half of the population is hard-working, and the other
half is lazy. Hard workers give it their all - with an effort of \\( 1 \\). Lazy
workers do nothing. The cost of effort \\( a = 0.5 \\).

![Oh dear. The first simulation doesn't look
promising.](/content/images/2017/06/differential-model-default-parameters-1.png)

Oh dear. That doesn't look promising. On the left-hand side, we see the average
*payoff* for each strategy - while on the right, we see the prevalence of each
strategy within the population. Within only eight semesters, pretty much
*everyone* becomes lazy. This is also reflected in the payoffs - pretty quickly,
the average payoff converges to zero. This is because hard workers are the only
individuals that actually contribute anything - and because they don't really
get a corresponding payoff for their efforts, the entire population stops
working hard pretty quickly.

[1]: http://users.ecs.soton.ac.uk/mb8/sim2017/sim.html

---
layout: post
title: Stochastic gradient descent
date: 2018-10-30 17:18:21.000000000 +01:00
---

Stochastic Gradient Descent (let's call it SGD from now on, because that feels a
bit less repetitive) is an online optimisation algorithm that's received a lot
of attention in recent years for several reasons (and is notable for its
applications in optimising neural networks). One of the most attractive things
about this algorithm is its sheer scalability - because of how it differs from
other gradient descent algorithms, SGD lends itself neatly to massively parallel
architectures.

<!-- more -->


## A recap of Gradient Descent

OK, so first off let's start with a recap of our good ol' buddy gradient
descent. Gradient descent is an iterative optimisation algorithm that takes
small steps towards a minimum of some function (notice that I say *a* minimum
instead of *the* minimum - GD can get caught in local minima if the starting
point is poorly chosen or the function is highly nonconvex). GD works by
evaluating the output of the objective function at a particular set of parameter
values - and then taking partial derivatives of the output with respect to each
of the parameters. Using these partial derivatives, it's possible to determine
which parameters to modify to minimise the value of the objective function.
Basically, the algorithm finds the direction that most minimises the
objective... and then takes a single step in that direction. Rinse and repeat
until convergence!

{% katexmm %}

### A quick peek under the bonnet...

That's the basic intuition covered, but let's take a little review of the maths.
The GD algorithm's objective is to minimise some function $J$ by determining the
optimal values of some parameter set $\theta$. This is done repeatedly, so a
single iteration of the algorithm (a single update of the parameters $\theta$)
would look something like this;

$$
\theta = \theta_{old} - \alpha \nabla_{\theta} E\left[ J(\theta, X, y) \right]
$$

The interesting thing to note here is that this requires computing the *expected
value* of the function $J(\theta, X, y)$ over the whole dataset. That is, the
objective function is evaluated over the entire dataset and then averaged - for
each iteration of the GD algorithm. This is fine for small optimisation tasks,
but this can hit significant problems at scale - as evaluating a function over a
whole dataset can prove an intractable task.


### An example with Linear Regression

Let's apply gradient descent to linear regression. Remember linear regression?
Minimising mean squared error? Mmkay, good. In case your memory is a little
fuzzy (like mine), let's look at the objective function for a linear regression
(with a grateful nod to Cosma Shalizi's excellent lecture notes on this
topic[^0]).

In this case, our learning problem has $n$ examples and $m$ features. Let's
define some things up front, so that our objective function makes sense to
read.

* Let $\underset{m \times n}{X}$ be our design matrix - with rows for each
  training example and columns for each feature (and an additional column for
  the bias term)
* Let $\underset{m \times 1}{y}$ be a vector holding our target variable - we
  want to predict $y$ given the data in our matrix $X$.
* Let $\underset{n \times 1}{\theta}$ be our parameter vector - an $n$-length
  vector containing the coefficients for each feature and the bias term.

In this case, the objective function over a dataset $X$ is defined as follows;

$$
J(\theta, X, y) = \frac{1}{2n} (y - \theta^{T}X)^{T}(y - \theta^{T}X)
$$

Note that sometimes machine learning practitioners divide this function by a
factor of two (i.e. the $\frac{1}{2m}$ at the beginning) so that the derivative
is a little bit neater. This isn't strictly necessary and doesn't change the
overall shape of the function at all - it's just a mathematical convenience.

To use gradient descent to optimise $J(\theta, X, y)$, we'd want to begin by
setting $\theta$ to some arbitrary values. We then evaluate the gradient
$J(\theta, X, y)$ at that point to get an idea of the general shape of the
function.  We then focus on taking a number of small steps "downhill" by
iteratively updating $\theta$ by nudging the weights in the direction that
minimises the objective. By taking enough steps this way, we will eventually
arrive at an optimum.

One thing to note here is that the linear regression objective function is
quadratic (like the one pictured below). It is therefore convex and has a
single global optimum --- this isn't always the case with more complex
functions.

![A quadratic function (like this) has a single global optimum.](/images/objective-function.png)


#### Figuring out which way is downhill

Let's compute the derivative for this objective function - once we've done that,
we can start stepping down the hill. This involves a little bit of matrix
calculus, so I've left some extra steps in so that it's clear what's happening.
Both Cosma Shalizi's lecture notes[^0] and the Matrix Cookbook[^3] are really
handy here.

$$
\begin{aligned}
\frac{\partial}{\partial \theta} J(\theta, X, y)
    & = \frac{\partial}{\partial \theta} \frac{1}{2n} (y - X\theta)(y - X\theta) \\
    & = \frac{1}{2n} \frac{\partial}{\partial \theta} \big( y^{T}y - 2\theta^{T}X^{T}y + \theta^{T}X^{T}X\theta \big) \\
    & = \frac{1}{2n} \big(-2X^{T}y + \frac{\partial}{\partial \theta} \theta^{T}X^{T}X\theta) \big) \\
    & = \frac{1}{2n} \big(-2X^{T}y + 2X^{T}X\theta \big) \\
    & = \frac{1}{n} \big(-X^{T}y + X^{T}X\theta \big) \\
    & = \frac{1}{n} \big(X^{T}X\theta - X^{T}y \big)
\end{aligned}
$$


#### Putting it all together

Now that the derivative of this function is known, we can plop it directly into
the gradient descent algorithm. The objective function that we determined
earlier computes the *expected value* over the entire dataset, so we can simply
drop it in place. A single round of parameter updates would look like this:

$$
\theta = \theta - \alpha \big( \frac{1}{n} X^{T}X\theta  - X^{T}y \big)
$$

I applied this parameter update to the Boston housing dataset (200 iterations
with a constant $\alpha = 0.05$), and charted the error below. You can see the
error begin to converge to a stable state after about 25 iterations;

![Error convergence after 200 iterations on the Boston housing
dataset](/images/gradient-descent-error.png)



## Now... back to SGD

Right. Now that we've recapped bog-standard gradient descent, let's dive right
into the interesting stuff. Luckily, you now know everything you need to
understand SGD because it's only a slight modification to the standard
algorithm. The update procedure for SGD is almost exactly the same as for
standard GD - except we don't take the expected value of the objective function
over the entire dataset, we simply compute it for *a single row*[^1].

$$
\theta = \theta_{old} - \alpha \nabla_{\theta} J(\theta, X_{(i)}, y_{(i)})
$$

As you may expect, this has some significant implications for setting the
learning rate. Typically (during the initial iterations, at least), parameters
($\theta$) in SGD will initially have far greater variance as they're being
updated directly with each training example. Over time, as the number of
training examples analysed grows larger, the influence of each individual
example diminishes and the parameters will start to converge to a steady state.


### Choosing a learning schedule

The sensitivity of the SGD algorithm to individual training examples means that
it's much more important to correctly choose the learning rate, $\alpha$ (this
isn't such a problem with other variants of the GD algorithm). Setting $\alpha$
too high means the algorithm won't converge smoothly (if at all), and setting it
too low means that it'll take far longer to converge than necessary.

There are several heuristic approaches for setting $\alpha$ that seem to be
relatively common in practice (and although there's fairly scant theoretical
justification for them, they seem to perform well). One such example is Leon
Bottou's heuristic[^2] (used in the SGD implementation in `sklearn`), while
several other learning schedules are mentioned in the [Stanford Deep Learning
materials][1] (this includes approaches like reducing the learning rate as the
algorithm begins to converge, or using an optimisation technique like simulated
annealing to periodically choose a better learning rate).


## Batch GD, Mini-Batch GD, SGD?


## Extra bits and caveats

### Scaling

One of the most important things to do before using SGD on your data is to
*scale* your input variables to have a zero mean and unit variance. This is as
straightforward as $X_{scaled} = \frac{(X - \bar{X})}{\sigma_X^{2}}$. If this
isn't done, this can have serious negative effects - in the best case scenario,
the gradient will be computed incorrectly and will cause slow convergence. In
the worst case, the gradient will run away with you (the "exploding gradient"
or "vanishing gradient") and stop the algorithm converging at all.


### Dataset ordering

One thing that can negatively impact SGD's performance is biased ordering in the
dataset. If there's some meaningful ordering to the data (e.g. horse racing
results where race winners are listed in the first row for each race), then this
can bias the gradients - if this happens over an appreciable number of training
examples, then this can cause the algorithm to choose vastly sub-optimal
parameter values. There are several ways around this, but the easiest solution
is simply to shuffle the dataset before using SGD.


### Momentum


## Confidence scores

["Decision function" in `sklearn`][8]


{% endkatexmm %}


[^0]: [Simple Linear Regression in Matrix Format; Cosma Shalizi (2015)][0]
[^1]: [Optimization: Stochastic Gradient Descent; Stanford Deep Learning][1]
[^2]: [The Tradeoffs of Large-Scale Machine Learning; Leon Bottou (2009)][2]
[^3]: [The Matrix Cookbook; Petersen & Pedersen (2012)][3]

[0]: https://web.archive.org/web/20181126144751/https://www.stat.cmu.edu/~cshalizi/mreg/15/lectures/13/lecture-13.pdf
[1]: https://web.archive.org/web/20181202225515/http://deeplearning.stanford.edu/tutorial/supervised/OptimizationStochasticGradientDescent
[2]: https://web.archive.org/web/20170125203645/https://istcolloq.gsfc.nasa.gov/fall2009/presentations/bottou.pdf
[3]: https://web.archive.org/web/20181202222329/https://www.math.uwaterloo.ca/~hwolkowi/matrixcookbook.pdf

[7]: http://archive.today/2018.11.28-102300/https://towardsdatascience.com/difference-between-batch-gradient-descent-and-stochastic-gradient-descent-1187f1291aa1
[4]: https://machinelearningmastery.com/gentle-introduction-mini-batch-gradient-descent-configure-batch-size
[5]: https://github.com/bfortuner/ml-cheatsheet
[6]: https://ml-cheatsheet.readthedocs.io/en/latest/gradient_descent.html
[8]: https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.SGDClassifier.html#sklearn.linear_model.SGDClassifier.decision_function

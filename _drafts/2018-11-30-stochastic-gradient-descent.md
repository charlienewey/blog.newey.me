---
layout: post
title: Stochastic gradient descent
date: 2018-10-30 17:18:21.000000000 +01:00
---

Stochastic Gradient Descent (let's call it SGD from now on, because that feels
a bit less repetitive) is an online optimisation algorithm that's received a
lot of attention in recent years for several reasons. One of the most
attractive things about this algorithm is its sheer scalability - because of
how it differs from other gradient descent algorithms, SGD lends itself
effortlessly to massively parallel architectures.

<!-- more -->


{% katexmm %}

## Gradient Descent

OK, so first off let's start with a recap of our good ol' buddy gradient
descent. Remember linear regression? Minimising mean squared error? Mmkay,
good. In case your memory is a little fuzzy (like mine), let's look at the
objective function for a linear regression (with a grateful nod to Cosma
Shalizi's excellent lecture notes on this topic[^1]).


### An example with Linear Regression

In this case, our learning problem has $m$ examples and $n$ features. Let's
define some things up front, so that our objective function makes sense to
read.

* Let $\underset{m \times n}{X}$ be our design matrix - with rows for each
  training example and columns for each feature (and an additional column for
  the bias term)
* Let $\underset{m \times 1}{y}$ be a vector holding our target variable - we
  want to predict $y$ given the data in our matrix $X$.
* Let $\underset{n \times 1}{\theta}$ be our parameter vector - an $n$-length
  vector containing the coefficients for each feature and the bias term.

In this case, the objective function can be phrased as follows;

$$
f(\theta) = \frac{1}{2m} (y - \theta^{T}X)^{T}(y - \theta^{T}X)
$$

Note that sometimes machine learning practitioners divide this function by a
factor of two (i.e. the $\frac{1}{2m}$ at the beginning) so that the derivative
is a little bit neater. This isn't strictly necessary and doesn't change the
overall shape of the function at all - it's just a mathematical convenience.

To use gradient descent to optimise $f(\theta)$, we'd want to begin by setting
$\theta$ to some arbitrary values. We then evaluate the gradient $f(\theta)$ at
that point to get an idea of the general shape of the function.  We then focus
on taking a number of small steps "downhill" by iteratively updating $\theta$
by nudging the weights in the direction that minimises the objective. By taking
enough steps this way, we will eventually arrive at an optimum.

One thing to note here is that the linear regression objective function is
quadratic (like the one pictured below). It is therefore convex and has a
single global optimum --- this isn't always the case with more complex
functions.

![A quadratic function (like this) has a single global optimum.](/images/objective-function.png)

Let's compute the derivative for this objective function - once we've done
that, we can start stepping down the hill.

$$
\begin{aligned}
\frac{\partial}{\partial \theta} J(\theta)
    & = \frac{\partial}{\partial \theta} \frac{1}{2m} (y - \theta^{T}X)^{2} \\
    & = \frac{1}{m} \cdot \frac{\partial}{\partial \theta} (y - \theta^{T}X) \\
    & = - \frac{1}{m} X^{T}(\theta^{T}X)
\end{aligned}
$$


## Batch GD, Mini-Batch GD, SGD?


## Definition

&theta; = &theta; - &alpha; &nabla;<sub>&theta;</sub> &Epsilon;[f(&theta;)]

* &theta; current value
* &alpha; (learning rate)
* f(&theta;) (expectation of cost function over dataset)
* &nabla;<sub>&theta;</sub> (Jacobian)


## Adaptive learning rates

[Leon Bottou's SGD heuristic][7]


## Confidence scores

["Decision function" in `sklearn`][8]


## In practice


{% endkatexmm %}


[^1]: [Simple Linear Regression in Matrix Format; Cosma Shalizi (2015)][9]
[1]: http://archive.today/2018.11.28-102444/http://deeplearning.stanford.edu/tutorial/supervised/OptimizationStochasticGradientDescent
[2]: http://ufldl.stanford.edu/tutorial
[3]: http://archive.today/2018.11.28-102300/https://towardsdatascience.com/difference-between-batch-gradient-descent-and-stochastic-gradient-descent-1187f1291aa1?gi=98a30d7be394
[4]: https://machinelearningmastery.com/gentle-introduction-mini-batch-gradient-descent-configure-batch-size
[5]: https://github.com/bfortuner/ml-cheatsheet
[6]: https://ml-cheatsheet.readthedocs.io/en/latest/gradient_descent.html
[7]: https://web.archive.org/web/20170125203645/https://istcolloq.gsfc.nasa.gov/fall2009/presentations/bottou.pdf
[8]: https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.SGDClassifier.html#sklearn.linear_model.SGDClassifier.decision_function
[9]: https://www.stat.cmu.edu/~cshalizi/mreg/15/lectures/13/lecture-13.pdf

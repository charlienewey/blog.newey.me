---
layout: post
title: An introduction to gradient boosting
date: 2018-12-13T09:00:00Z
---

Well, it's pretty much all in the title. Gradient boosting is one of those
unavoidable algorithms if you're a data science professional - it's very common,
fast, lightweight, and capable of exceptional accuracy when tuned correctly.
There's been renewed enthusiasm for gradient boosting algorithms with the advent
of popular frameworks like XGBoost and LightGBM, and they get used for almost
everything - so much so that it's difficult to find a Kaggle competition winner
that doesn't use some form of gradient boosting.

<!-- more -->

Today, we'll take a leisurely stroll through some of the fundamentals of
gradient boosting; the ideas that served as the basis for GB, the motivation for
using gradients in an ensemble learning model, and how all of the basic parts of
the gradient boosting algorithm fit together. My hope is that this post will
give you a solid intuition for what gradient boosting is for and how it all
works.  Let's get started.


## Introduction to Boosting

Boosting algorithms started to appear in machine learning literature in the
early-to-mid-1990s (around the same time as other ensemble methods like bagging)
as a way of addressing some of the shortcomings in simpler models (high
variance, tendency to overfit, etc). The idea of ensemble methods was to combine
a number of "weak learners" - also referred to as "probably approximately
correct" (PAC) models - such that several of them can be combined into a more
complex model of arbitrarily high accuracy.

Freund and Schapire[^1] make a nice analogy here, and I'll lift a quote from
their paper;

> A horse-racing gambler, hoping to maximize his winnings, decides to create a
> computer program that will accurately predict the winner of a horse race.
> [...]
> When presented with the data for a specific set of races, the expert has no
> trouble coming up with a "rule of thumb" for that set of races (such as, "bet
> on the horse that has recently won the most races" or "bet on the horse with
> the most favored odds"). Although such a rule of thumb, by itself, is
> obviously very rough and inaccurate, it is not unreasonable to expect it to
> provide predictions that are at least a little bit better than random
> guessing.

In theory, almost any PAC model can be used as a "base learner" in gradient
boosting - but note that, in practice, decision trees are *almost always* the
chosen algorithm. I suspect this is for several reasons; trees are *really*
fast to train, it's very easy to restrict their complexity (e.g. limiting depth
or the number of leaves), and they're pretty lightweight in terms of memory
requirements - which makes them ideal for using in ensemble.


### A bit of history: AdaBoost

AdaBoost was one of the first popular boosting algorithms, coined by Freund and
Schapire[^2] in 1995, and deserves an honourable mention in this post, as it . AdaBoost
is an iterative algorithm that builds an additional classifier on each
iteration, increasing the weights of examples that previous algorithms
mis-classified (hence the name AdaBoost - "adaptive boosting"). Each subsequent
iteration makes AdaBoost focus on things it got wrong - that is, each additional
model is designed to correct the mistakes made by the model before it.

{% katexmm %}

AdaBoost starts by defining a probability distribution $D _ t$ over all data
points $x _ {i} \in X$. Initially, this distribution is uniform (i.e. all points
are equally likely), but this distribution is updated on each iteration with the
intention of making AdaBoost focus on examples that are the "trickiest" to
classify.

An additional hypothesis $h _ {t}(x)$ is added to the ensemble on each iteration
(trained on the dataset weighted according to $X \sim D _ {t}$), and the error
is then evaluated on the updated ensemble. The error for each example is
accumulated in a vector $\epsilon _ {t}$, given by $\epsilon _ {t} = Pr _ {i
\sim D _ {t}}$ (this is a *weighted sum of the errors* of each training example,
with the weights given by the probability distribution $D _ {t}$). Finally, the
distribution $D _ {t}$ is updated and normalised (where $Z _ {t}$ is a constant
s.t. chosen $D _ {t+1}$ integrates to 1).

$$
\begin{aligned}
    \alpha _ {t} &= \frac{1}{2} \ln \big( \frac{1 - \epsilon _ {t}}{\epsilon _
    {t}} \big) \\
    D _ {t+1} &= D _ {t} \frac{\exp\big(-\alpha _ {t} y h _ {t}(x)\big)}{Z _ {t}}
\end{aligned}
$$

{% endkatexmm %}

As it turns out, the AdaBoost algorithm is equivalent to building an additive
model in a forward stagewise fashion[^3] - an additive model that iteratively
minimises an exponential loss function. This is a neat interpretation of the
AdaBoost algorithm, as forward stagewise optimisation is the foundation upon
which Gradient Boosting is built. Gradient boosting improves on the core ideas
of AdaBoost by allowing more complex objective functions to be optimised.


### Gradient Boosting


## References

[^1]: [Freund and Schapire, "A Short Introduction to Boosting"][1]
[^2]: [Freund and Schapire, "A decision-theoretic generalization of on-line learning and an application to boosting"][2]
[^3]: [Gu, "AdaBoost Fits an Additive Model"][3]

[1]: https://web.archive.org/web/20180403173111/https://cseweb.ucsd.edu/~yfreund/papers/IntroToBoosting.pdf
[2]: https://web.archive.org/web/20180820053835/https://cseweb.ucsd.edu/~yfreund/papers/adaboost.pdf
[3]: https://web.archive.org/web/20151106135151/http://www.cs.cmu.edu/~epxing/Class/10701-08s/recitation/boosting.pdf

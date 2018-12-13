---
layout: post
title: An introduction to gradient boosting
date: 2018-12-13T09:00:00Z
---

Well, it's pretty much all in the title. Gradient boosting is one of those
unavoidable algorithms if you're a data science professional - it's very
common, fast, lightweight, and capable of exceptional accuracy when tuned
correctly. It's now so ubiquitous that it's difficult to find a Kaggle
competition-winning model that doesn't use some form of gradient boosting.

<!-- more -->

Today, we'll take a leisurely stroll through some of the basics of gradient
boosting; the algorithms that served as the inspiration for GB, the motivation
for using gradients in an ensemble machine learning model, and how all of the
component parts of the algorithm fit together. My hope is that this post will
give you a solid intuition for what gradient boosting is for and how it all
works. Let's get cracking.


## Introduction to Boosting

Boosting algorithms started to appear in machine learning literature in the
early-to-mid-1990s (around the same time as other ensemble methods like
bagging). The idea of ensemble methods was to combine a number of "weak
learners" - also referred to as "probably approximately correct" (PAC) models -
such that several of them can be combined into a more complex model of
arbitrarily high accuracy.

Freund and Schapire[^1] make a nice analogy here, and I'll quote from their
paper;

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
requirements - which makes them ideal for using in an ensemble.


### AdaBoost

AdaBoost was one of the first popular boosting algorithms, coined by Freund and
Schapire[^2] in 1995. AdaBoost is an iterative algorithm that builds an
additional classifier on each iteration, increasing the weights of examples
that previous algorithms mis-classified (hence the name AdaBoost - "adaptive
boosting"). Each subsequent iteration makes AdaBoost focus on things it got
wrong - that is, each additional model is designed to correct the mistakes made
by the model before it.

As it turns out, the AdaBoost algorithm is equivalent to building an additive
model in a forward stagewise fashion[^3]. This is a neat phrasing of the
AdaBoost algorithm, as forward stagewise optimisation is the foundation upon
which Gradient Boosting is built. Gradient boosting improves on the core ideas
of AdaBoost by generalising the ideas within it and allowing more complex
objective functions to be optimised.


## References

[^1]: [Freund and Schapire, "A Short Introduction to Boosting"][1]
[^2]: [Freund and Schapire, "A decision-theoretic generalization of on-line learning and an application to boosting"][2]
[^3]: [Gu, "AdaBoost Fits an Additive Model"][3]

[1]: https://web.archive.org/web/20180403173111/https://cseweb.ucsd.edu/~yfreund/papers/IntroToBoosting.pdf
[2]: https://web.archive.org/web/20180820053835/https://cseweb.ucsd.edu/~yfreund/papers/adaboost.pdf
[3]: https://web.archive.org/web/20151106135151/http://www.cs.cmu.edu/~epxing/Class/10701-08s/recitation/boosting.pdf

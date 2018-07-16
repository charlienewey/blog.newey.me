---
layout: post
title: Just how bad is London's homicide rate?
date: 2018-04-16 00:09:55.000000000 +01:00
---

There's been [a lot][std] [in the news lately][bbc] about the supposedly
"soaring" murder rate in London, and how it has supposedly eclipsed that of New
York City in recent months. I was initially sceptical as this fact seemed fairly
mundane --- 50 homicides in 3-and-a-bit months didn't seem all that unlikely.
However, I wanted put my scepticism to the test --- so I acquired a couple of
datasets from the [London Datastore][data] to see if the hyper-sensationalised
news articles were justified or not.

[std]: https://www.standard.co.uk/news/crime/the-55-murder-investigations-launched-in-london-this-year-as-death-toll-continues-to-rise-a3807186.html
[bbc]: http://www.bbc.co.uk/news/uk-england-london-43610936
[data]: http://archive.is/APgSj

<!-- more -->

# How is the data distributed?

![Distribution plot of data](/images/distribution_kde.png)

With the data being homicide **counts** per month, it seems probable that the
data are generated according to an exponential model (or, at least, something
that we can model using a Poisson process). However, for the sake of due
diligence, let's see if the data comes from a normal distribution. We'll use the
Shapiro-Wilks, D'Agostino-Pearson, and Anderson-Darling tests for this.

The Shapiro-Wilks test appears to suggest that we can **reject** the null
hypothesis with a reasonable level of confidence at $p = 0.018$ (the null
hypothesis being that the data are distributed normally). This implies that
there is a reasonable probability that the data are not from a normal
distribution (as we expected).

```
p = 0.02, therefore reject null hypothesis
```

The D'Agostino-Pearson test is a hypothesis test for normality, where the null
hypothesis is that the data are normally distributed. Applying the test yields a
p-value of 0.18 - this is reasonably high, so we can't rule out the normal
distribution completely.

```
p = 0.18, therefore can't reject null hypothesis
```

The final test we'll use is the Anderson-Darling. This is very similar to the
Kolmogorov-Smirnov test, but places greater emphasis on the distribution's
tails.

```
A² = 0.87
critical value at p = 0.15 is 0.56. can reject the null hypothesis.
critical value at p = 0.10 is 0.64. can reject the null hypothesis.
critical value at p = 0.05 is 0.76. can reject the null hypothesis.
critical value at p = 0.03 is 0.89. can't reject the null hypothesis at this significance.
```

So, we've amassed a reasonable body of evidence to suggest that the data is
likely to be non-normally distributed - as we expected. In the absence of any
better ideas, let's go ahead and try to fit a Poisson distribution to the data
to see if we can discover any useful information.


# What are the parameters of this distribution?

![Poisson CDF of data](/images/cdf_ecdf.png)

This chart shows the empirical cumulative distribution for the homicide data -
this simply fits a step function to the data based on the observed probability
of each value. Plotted next to it is the cumulative distribution function for
the Poisson distribution that was estimated using maximum likelihood (i.e.
$\lambda = \mu$, the mean of the homicide data). With a p-value of $0.42$, it
seems very possible that the observed data come from a Poisson distribution.

```
χ2 = 15.438520075519012, p = 0.4203150760901015
```


# Just how unlikely *is* this event?

Now that we've characterised our distribution as something sensible, let's see
if the current reported figures for homicides are *really that unlikely*. The
headline figure that most news media sources have been touting tends to be along
the lines of "$50$ murders in London in the first 3 months of 2018", or "soaring
murder rates in the capital" and so on. Actually (as can probably be expected),
these headlines aren't quite right, and the reality of the situation is a little
bit less dramatic - but *only a little bit*.

Firstly, we need to clarify our terminology. The figures released by the
Metropolitan Police are related to *homicides* - not *murders*. A homicide is a
broader category of offence that can include manslaughter, corporate
manslaughter, murder, and so on. So it's not necessarily all about murders - but
of course, we're splitting hairs here.

Linguistic nit-picking aside, the most interesting insights come from looking at
the raw data. According to [this article from the BBC][1], $46$ homicides
happened in London between 00:00 on January 1st, 2018 and 23:59 on March 31st,
2018. Most news media have reported on $50$ being the key figure simply because
it's a round number.

46 homicides in 3 months implies an average count of $\frac{46}{3} \approx 15.3$
per month. For there to be 46 homicides in 3 months, this implies 3 consecutive
months of $15.3$ homicides (or more). Crunching the numbers gives us a
probability of $\approx 0.043$ - or about $1$ in $25$. The probability of this
occurring for three consecutive months is extremely low - in fact, this will
only happen once in about every $1041$ years.

[1]: http://www.bbc.co.uk/news/uk-43640475


```
p(x >= 15.3) = 0.04
probability of x >= 15.3 for 3 consecutive months is 8.004967e-05
which would happen every 1041.02 years on average.
```

However, this perhaps isn't the best way of working this out. The model above
only takes into account the probability of  months with only $15.3$ or more
homicides... Whereas in reality a count of $46$ homicides per 3 month period
could be composed of one month with $17$ homicides, one with $18$, and one with
$11$. Actually, this is exactly what happened in London at the beginning of 2018
--- there were $11$ homicides in January, $15$ in February, and $20$ in March;
for a total of $46$.

This eventuality wouldn't be accounted for in the model above, so let's try a
different approach. Let's count the number of homicides *per three month period*
between 2008 and 2017, and see if it's possible to find any particular
three-month periods with over $46$ homicides.

It turns out that the probability of $46$ homicides in any three-month period is
about $1.6\times10^{-3}$ - or about $\frac{1}{625}$. Given that there are
*twelve* possible rolling three-month periods in a year (think about it;
*Jan/Feb/Mar*, *Feb/Mar/Apr*, ..., *Nov/Dec/Jan*, etc), there is a chance of
about $\frac{1}{52}$ that a single three-month period with $\geq 46$ homicides
will occur in a given year.

Another way to put this is that this phenomenon is likely to occur once every
$52$ or so years. Very uncommon, but not altogether unexpected every once in a
while.

```
Three-month period: p(x >= 46) = 0.0016
Annual: p(x >= 46) = 0.0191
This is likely to happen approx. once every 52.35 years
```

This is clearly a very uncommon event, but it isn't outside of the realms of
possibility. Statistical anomalies happen all the time and there's *always*
going to be some city or other throughout the world experiencing this kind of
phenomenon.

My bet? We'll see a regression to the mean over the next month or two and this
won't be mentioned in the media for another $52$ years (or thereabouts!) - after
all, it's probably just statistical noise.

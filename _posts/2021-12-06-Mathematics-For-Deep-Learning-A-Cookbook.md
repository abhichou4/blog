---
toc: true
layout: post
description: Linear Algebra, Probability and Information theory used in Deep Learning
categories: [Mathematics, Deep Learning] 
title: Mathematics for Deep Learning - A Cookbook
---

This blog post serves as a quick review for the mathematics of my Deep Learning class.


## Probability

Probability can crudely be of two kinds:

1. Frequency Probability
2. Bayesian probability

But with a small set of common sense assumptions, we treat bayesian probability to be exactly same as frequency probability.

### Random Variables

Variables that can take different values randomly. They can be used as descriptors of the states that are possible and how likely they are.

They can be continuous or discrete. Set of states in denoted by $\bold{x}$ and individual values as ${x_1}, {x_2}$, etc.

### Probability Mass Functions (PMF)

Probability Mass Function (PMF) maps the discrete state of the random variable to it's probability. It's usually denoted as $P(\bold{x}$.

There are there conditions for a PMF must satisfy:

1. The domain of P must be the set of all possible states of $\bold{x}$
2. $\forall x \in \bold{x}, 0 \leq P(x) \leq 1$
3. $\forall {x} \in \bold{x}, \sum P(x) = 1$


### Probability Density Functions (PDF)

When the states are continuous, Probability Density Functions maps real values of random variable to probability of states. It's usually denoted by p.

Analogous to PMF, the conditions for a PDF are:

1. The domain of p must be set of all possible states of x
2. $\forall x \in \bold{x}, p(x) \geq 0$ only
3. $\int p(x) \, dx = 1$

### Common Operations on PMFs and PDFs

#### Marginal Probability

Remove one dimension of dependency by summing across it i.e. marginalize it.

$\forall x \in \bold{x}, P(\bold{x} = x) = \displaystyle\sum_y P(\bold{x} = x, \bold{y} = y)$

and

$p(x) = \int p(x, y) \, dy$

#### Conditional Probability

Computes the probabilty of one event happening given that another has happened.

$P(\bold{y} = y \mid \bold{x} = x) = \frac{\displaystyle P(\bold{y} = y, \bold{x} = x)}{\displaystyle P(\bold{x} = x)}$

It can only be computed if $P(\bold{x}) > 0$.

Note: This is not the same as what would happen if some action was taken. Those are inversion queries in the domain of causal modeling.

### The Chain Rule of Conditional Probability

Using the above, any joint probability distribution over many random variables may be decomposed into conditional distributions over only one variable.

$P(\bold{x} ^{(1)},..., \bold{x} ^{(n)}) = P(\bold{x} ^{(1)}) \prod\limits_{i=2}^n P(\bold{x} ^{(i)} \mid \bold{x} ^{(1)},..., \bold{x} ^{(i-1)})$

### Independence and Conditional Independence

Random variable $x$ and $y$ are independent if their joint probability distribution can be expressed as a product of their individual probability distribution.

$p(\bold{x} = x, \bold{y} = y) = p(\bold{x} = x) \cdot p(\bold{y} = y)$

Conditional independence is independence, given another random variable $z$.

$p(\bold{x} = x, \bold{y} = y \mid \bold{z} = z) = p(\bold{x} = x \mid \bold{z} = z) \cdot p(\bold{y} = y \mid \bold{z} = z)$

### Expectance, Variance and Covariance

The expectation of expected value of a function $f(x)$ is the mean or average value it takes when the random variable $x$ is drawn from the probability distribution $P(x)$

$E_{x \sim P(x)}[f(x)] = \displaystyle\sum_x P(x)f(x)$ 

$E_{x \sim p(x)}[f(x)] = \displaystyle\int p(x)f(x) \, dx$

Expectations are linear i.e.

$E[\alpha f(x) + \beta g(x)] = \alpha E[f(x)] + \beta E[g(x)]$

if $\alpha$ and $\beta$ are independent of $x$.

Variance is a measure of how much the value deviates from the expected values.

$Var(f(x)) = E[f(x) - E[f(x)]]$

Square root of variance is the standard deviation.

The covariance gives some sense of how much two function values are linearly related to each other, as well as the scale of these values.

$Cov(f(x), g(x)) = E[(f(x) - E[f(x)])(g(x) - E[g(x)])]$

* High absolute value means the values change very much and are far from their respective means at the same time (their scale).
* If positive, both functions tends to take high values simultaneously (linear relation).
* If negative, one function tends to take higher values while the other takes lower and vise versa (linear relation).

Correlation normalizes each variable in order to measure relation independent of scale.

* Two functions on random variables can have zero covariance, and still be dependent. Eg: $f(x) = x, x \in U[-1, 1]$ and $g(x) = sx$ where $s$ is a random variable with $1/2$ probability to be $1$ and rest to be $-1$. Although $g(x)$ is clearly dependent on $x$, $Cov(f(x), g(x)) = 0$. The reason it is not the sufficient condition for independence is it doesn't account for non-linear relationship.
* But if covariance is non-zero, the two functions have some linear dependency.

Covariance for a random vector $\bold{x} \in \mathbb{R}^n$ is an $N \times N$ matrix such that

$Cov(x)_{i, j} = Cov(x_i, x_j)$

and diagonal elements of the covariance matrix give the variance:

$diag(Cov(x)) = Var(x)$ 

### Common Probability Distributions

#### Discrete Probability Distribution

Bernoulli and Multinoulli distributions are enough to describe any discrete distribution over their domain, since the domain is very simple.

##### Bernoulli Distribution

It's a distribution over a single binary variable. It's main parameter is $\phi \in [0, 1]$.

$P(x = 1) = \phi$ \
$P(x = 0) = 1 - \phi$ \
$x \in [0, 1], P(\bold{x} = x) = \phi ^ x (1 - \phi) ^ {1 -x}$ \
$E[x] = \phi$ \
$Var(x) = \phi(1 - \phi)$

##### Multinoulli Distribution

It's a distribution over a single discrete variable with k different states, where k is finite. It is parameterize by a vector $\bold{p} \in [0, 1] ^ k -1$, where $p_i$ is the probability of $ith$ state. The $kth$ state probability is $1 - \bold{1}^T \bold{p}$. Therefore, $\bold{1}^T \bold{p} \leq 1$. Since the probabilities aren't defined, expectation and variance are not calculated for this distribution. 

#### Continuous Probability Distributions

When dealing with continuous distributions, there are infinite states, so any distribution with small number of parameters must impose strict limits on the distribution.

##### Gaussian Distribution

The most common distribution over real numbers is the gaussian distribution.

$\mathcal{N}(x; \mu, \sigma^2) = \sqrt{\displaystyle\frac{1}{2 \pi \sigma^2}} \Bigg(\displaystyle\frac{-1}{2 \sigma^2} (x - \mu)^2\Bigg)$

Where $\mu$ is the mean and $\sigma^2$ is the standard deviation for the distribution. 
An even better way to parameterize the normal distribution is using precision or inverse variance $\beta$:

$\mathcal{N}(x; \mu, \beta^-1) = \sqrt{\displaystyle\frac{\beta}{2 \pi}} \Bigg(\displaystyle\frac{-\beta}{2} (x - \mu)^2\Bigg)$

Standard normal distribution is with $\mu = 0$ and $\sigma = 1$

In the absence of prior knowledge about distribution for a random variable, normal distribution is a good choice for two main reasons:

1. The central limit theorem shows that the sum of many independent random is approximately normal distribution. So many complicated systems can be modelled as normal distribution noise.
2. Out of all possible probability distributions with same variance, normal distribution encodes maximum about of uncertainity and least amount of prior.

#### Exponential and Laplace Distributions

#### The Dirac Distribution and Empirical Distribution

#### Mixtures of Distribution

Status: Work in Progress.

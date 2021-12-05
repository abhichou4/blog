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
3. $\sum_{{x} \in \bold{x}} P(x) = 1$


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

$P(\bold{y} = y | \bold{x} = x) = \frac{\displaystyle P(\bold{y} = y, \bold{x} = x)}{\displaystyle P(\bold{x} = x)}$

It can only be computed if $P(\bold{x}) > 0$.

Note: This is not the same as what would happen if some action was taken. Those are inversion queries in the domain of causal modeling.

### The Chain Rule of Conditional Probability

Using the above, any joint probability distribution over many random variables may be decomposed into conditional distributions over only one variable.

$P(\bold{x} ^{(1)},..., \bold{x} ^{(n)}) = P(\bold{x} ^{(1)}) \prod\limits_{i=2}^n P(\bold{x} ^{(i)} | \bold{x} ^{(1)},..., \bold{x} ^{(i-1)})$

### Independence and Conditional Independence

Random variable $x$ and $y$ are independent if their joint probability distribution can be expressed as a product of their individual probability distribution.

$p(\bold{x} = x, \bold{y} = y) = p(\bold{x} = x) \cdot p(\bold{y} = y)$

Conditional independence is independence, given another random variable $z$.

$p(\bold{x} = x, \bold{y} = y | \bold{z} = z) = p(\bold{x} = x | \bold{z} = z) \cdot p(\bold{y} = y | \bold{z} = z)$


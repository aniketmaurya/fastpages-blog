---
title: Probability vs Likelihood vs Maximum Likelihood
description: Theory and examples of Probability vs Likelihood vs Maximum Likelihood
categories: [Statistics, Machine Learning]

toc: true
layout: post
comments: false
image: https://raw.githubusercontent.com/aniketmaurya/machine_learning/master/blog_files/2020-04-29-Probability-vs-Likelyhood/dice-min.jpg

keywords: machine learning, deep learning, statistics, autoencoders, ga
---

![](https://raw.githubusercontent.com/aniketmaurya/machine_learning/master/blog_files/2020-04-29-Probability-vs-Likelyhood/dice-min.jpg "Photo by Riho Kroll on Unsplash")

# Article is in progress 2020-04-30
# Probability
Probability is the quantitative measure of certainty of an event.
Mathematically, probability $P$ for an event $E$ is defined as:

$P(E) = \frac{number\ of\ times\ event\ E\ occurred}{Total\ number\ of\ events}$


Consider a fair die, possible outcomes of rolling the die can be 1 to 6. Where probability of each outcome is 1/6.
<bold>$P(1) = P(2) = P(3) = P(4) = P(5) = P(6) = 1/6$</bold>

**What if we roll two dice together? What will be the probability that both dice will get the same digits, say 6?**

<small>
$P(dice_1=6\cap dice_2=6) = P(dice_1=6) \times P(dice_2=6)=\frac{1}{6} \times \frac{1}{6}$
</small>

**What if one of the dice is biased towards 6, i.e.** <bold> $P(dice_1=6) = 1/4\ and\ P(dice_2=6)=1/6$</bold>

Our probability will become <small>$P(dice_1=6\cap dice_2=6) = 1/6 \times 1/4$</small>

## Joint Probability
Joint probability is the probability of two events occurring simultaneously.
Rolling two dice together is an example of Joint Probability.

If the two events E1 and E2 are independent of each other then the joint probability is multiplication of P(E1) and P(E2).

> $P(A\ given\ B)$ is denoted as $P(A\|B)$

<small>$P(E_1\ and\ E_2) = P(E_1) \times P(E_2) = P(E_1|E_2)P(E_2) = P(E_2|E_1)P(E_1)$</small>


If the two events E1 and E2 are not independent then the joint probability is:

<small>$P(E_1\ and\ E_2) = P(E_1|E_2)P(E_2) = P(E_2|E_1)P(E_1)$</small>


## Conditional Probability
It is the probability of one event when the other is given.

Probability of getting a 6 given that the die is biased one, that is P(6) = 1/4






## Maximum Likelihood
> Maximum Likelihood is used to find the normal distribution of the given data. We estimate $\mu$ and $\sigma$ for the distribution.






# References
* https://machinelearningmastery.com/joint-marginal-and-conditional-probability-for-machine-learning/

* https://towardsdatascience.com/probability-concepts-explained-maximum-likelihood-estimation-c7b4342fdbb1
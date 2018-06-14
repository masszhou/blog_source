---
title: Bayes Inference by Hand
date: 2016-09-30 14:07:30
tags: probability theory
---

{% centerquote %}Too much information every day, I have to backup some sections of my brain here.{% endcenterquote %}

# An interesting quiz

In a course by Prof. Isbell and Prof. Littman, I found a quiz as quite an good example to understand Bayes inference. Hence I post my deduction here as an archive.

The quiz is, there are two black boxes. In box 1 there are 3 green balls and 1 orange ball. In box 2 there are 3 blue balls and 2 green balls. The question is, when we randomly picked balls from one unknown box, we noticed that the first ball is green, what is the probability that the second picked ball is blue. Note, here $Pr(box=1)=Pr(box=2)=0.5$ is given.

<img src="https://www.dropbox.com/s/izkj3cqzs2lrgug/Screen%20Shot%202016-09-30%20at%2014.10.42.png?dl=1" alt="Drawing" style="height: 150px;"/>

<!-- more-->

*Here we denote x=1,2 for the box index. b1,b2 stand for first and second picked ball. g,b,o stand for the color of ball.*

The question can be formulated as
$$
\Pr(b2=b \mid b1=g) = ?
$$

Before we begin to solve this quiz, let's first have a short review about important tools in Bayes inference

# Toolchains
>Joint probability is the ultimate cheat card. We can answer anything once we know the joint probability.

## Marginalizing
{%math%}
\begin{equation*}
\Pr(X)=\sum_Y \Pr(X, Y) \tag{1} \label{1}
\end{equation*}
{%endmath%}

Below is an example from Wikipedia

<img src="https://www.dropbox.com/s/jchhwswtmgrf25y/Screen%20Shot%202016-09-30%20at%2014.53.34.png?dl=1" alt="Drawing" style="height: 250px;"/>

## Chain rule
{%math%}
\begin{equation*}
\Pr(A,B) = \Pr(A \mid B)\Pr(B) = \Pr(B \mid A)\Pr(A) \tag{2} \\
\Pr(A,B,C) = \Pr(A \mid B,C)\Pr(B,C) = \Pr(A \mid B,C)\Pr(B \mid C)\Pr(C)
\end{equation*}
{%endmath%}

for conditional probability

{%math%}
\begin{align*}
\Pr(A,B \mid C) &= \frac{\Pr(A,B,C)}{\Pr(C)} \\
&= \frac{\Pr(A \mid B,C) \Pr(B, C)}{\Pr(C)} \\
&= \frac{\Pr(A \mid B,C) \Pr(B \mid C) \Pr(C)}{\Pr(C)} \\
&= \Pr(A \mid B,C) \Pr(B \mid C) \tag{3} \label{3}
\end{align*}
{%endmath%}


# The quiz

We have two ways to solve this problem. One is using the exactly tools we have to deduct the results, no matter the concrete problem is. Another is analyzing the problem with intuition.

## Deduction with formula

{%math%}
\begin{align*}
\Pr(b2 = b \mid b1 = g) &= \sum_x \Pr(b2=b, x \mid b1=g) && \text{by \eqref{1}} \\
&= \Pr(b2=b, x=1 \mid b1=g) + \Pr(b2=b, x=2 \mid b1=g) \\
&= \Pr(b2=b, x=2 \mid b1=g) &&\text{no blue ball in box 1}\\
&= \Pr(b2=b \mid x=2, b1=g) \Pr(x=2 \mid b1=g) && \text{by \eqref{3}} \tag{4}\\
\end{align*}
{%endmath%}

where
{%math%}
\begin{align*}
\Pr(x=2 \mid b1=g) &= \frac{\Pr(b1=g \mid x=2)\Pr(x=2)}{\Pr(b1=g)} \\
&=\frac{\Pr(b1=g \mid x=2)\Pr(x=2)}{\Pr(b1=g \mid x=1)\Pr(x=1)+\Pr(b1=g \mid x=2)\Pr(x=2)} \\
&= \frac{3/5 \times 1/2}{3/4 \times 1/2 + 3/5 \times 1/2} \\
&= \frac{12}{27} \tag{5}
\end{align*}
{%endmath%}

---
title: Intuition behind Lagrange Duality
date: 2016-09-10 02:27:23
tags: optimization
---

## 1. Mysterious "dual"
Suppose we are going to solve the optimization problem
{%math%}
\begin{equation}
\min_x f(x) \\
s.t. \space h_i(x) \leq 0, \space i=1 \ldots m \\
\mathcal{D}=\text{dom }f\cap \bigcap_{i=1}^{m}\text{dom }h_i
\end{equation}
{%endmath%}

*note, the equation constrains can be represented by two inequation constrains. To simplify our problem, here we use only inequation constrains. Besides, all the functions we discussed in this blog are nicely functions, they are always continuous and derivable, as whatever you need.*

Due to the standard procedure, we firstly construct a Lagrangian function, which can be considered as adding constrains as perturbation terms.
{%math%}
\begin{equation}
\mathcal{L}(x, \lambda)=f(x)+\lambda^T h(x), \space with \space \lambda \succeq 0
\end{equation}
{%endmath%}

Then we define a "mysterious" dual function as
{%math%}
\begin{align}
g(\lambda) &= \inf_{x\in \mathcal{D}} (\mathcal{L}(x, \lambda)) \\
&= \inf_{x\in \mathcal{D}}(f(x)+\lambda^T h(x))
\end{align}
{%endmath%}

*note, to get a infimum, x is still in the domain $\mathcal{D}$, not feasible regions from constrains.*

Finally, by solving the dual problem
{%math%}
\begin{equation}
\max_{\lambda} g(\lambda) \quad s.t. \space \lambda \succeq 0
\end{equation}
{%endmath%}

We can get a satisfying lower bound of the primal problem.

<!-- more-->

Here is a short proof about $g(\lambda)$ is the lower bound of primal problem.
{%math%}
\begin{align}
g(\lambda) &= \inf_{x\in \mathcal{D}} (\mathcal{L}(x, \lambda)) \\
&\leq f(x)+\lambda^T h(x) \\
&\leq f(x) && \text{if x in feasible region}
\end{align}
{%endmath%}

These are what we learned from text book, surely there are also deduction and proofing.

However it's difficult for me to explain these to other people, when they keep asking why, because I didn't have a clear intuition about this standard procedure. For myself, the most agonizing questions were all about this dual problem. The algorithm is clear, but why we choose to solve the dual problem to estimate the lower bound of primal problem instead of solving primal problem directly. What is the intuition behind this dual problem?

In the following sections, I will try to explain my intuitive understanding. At the same time I will try to avoid the mathematic proofs, which is beautiful and logically perfect but hard for mortal to speak.

Be careful, all the examples in this blog are special cases, which are helpful to understand the intuition, but since they are special cases, their properties can not be simply generalized. If I made any mistakes, please notify me to correct, thank you.

## 2. Why Lagrangian function

> Why we need to cnstruct this Lagrangian function ?

> The answer is about perturbation

We can not solve the primal problem by just calculating $\frac{\partial{f(x)}}{\partial{x}}=0$, because of the feasible region of constrains. One possible solution is adding the constrains as perturbation terms, and then estimate the best lower bound as Eq.2 shows.

Here is an example. The optimization problem is
{%math%}
\begin{equation*}
\min_x x² \\
s.t. \space x-1 \leq 0 \\
\mathcal{D}=\mathbb{R}
\end{equation*}
{%endmath%}

The optimal solution is easy to guess, which is $p^{\star}=0$, when $x=0$.

The Lagrangian function is
$$
\mathcal{L}(x, \lambda)=x^2+\lambda (x-1), \space with \space \lambda \geq 0
$$

The dual function is
$$
g(\lambda) = \inf_{x \in \mathcal{D}}(x^2+\lambda (x-1)), with \space \lambda \geq 0
$$

consider $\inf_{x \in \mathcal{D}}(x^2+\lambda (x-1))$ is a function about x, the infimum exists only when $\frac{\partial{\mathcal{L}(x,\lambda)}}{\partial{x}}=0$.
Therefor we have $x=-\frac{\lambda}{2}$. plug this back to the dual function, we have
$$
g(\lambda)=-\frac{1}{4}\lambda^2-\lambda, with \space \lambda \geq 0
$$

And the dual problem is
$$
\max_{\lambda} g(\lambda) \quad s.t. \space \lambda \geq 0
$$
And the dual solution is $d^{\star}=0$, with $\lambda=0$

Now let's plot the solution with intuition. In Fig.1, the black curve is our original problem, blue and green curve is original problem with perturbations as Lagrange function. We can see the values of $inf_{x \in \mathcal{D}} \mathcal{L}(x, \lambda)$, e.g. saddle points on blue and green curve, are always equals or smaller than the optimal solution $p^{\star}$, which illustrates $g(\lambda)$ is a lower bound of $f(x)$.

The red curve represents the dual function $g(\lambda)$. It is easily to find out, when $\lambda = 0$, $g(\lambda)$ achieve its maximum, and the $\max_{\lambda}g(\lambda)$ exactly equals to the $\min_x f(x)$.

<img src="https://www.dropbox.com/s/0qbij8ef1teu1sg/Screenshot%20from%202016-09-11%2016-18-41.png?dl=1" alt="Drawing" style="height: 400px;"/>

*Fig.1 Lagrangian function and dual function. note, $f(x)$ and $g(\lambda)$ have independent variables, for a more concise expression, the red curve should be plotted on an additional figure.*

This example and figure give us a simple intuition about how the optimization works. But are there any other reasons, that we prefer to find the lower bound by solving its dual problem?

## 3. Why dual problem

>Why we choose to solve the dual problem to estimate the lower bound? is it easier to solve?

>The answer is YES. It is easier to solve.

In fact dual function $g(\lambda)$ is concave, or we say $-g(\lambda)$ is convex.

From Eq.(3), we know $-g(\lambda)$ is pointwise supremum of a set of affine functions about $\lambda$, which means $-g(\lambda)$ is convex. About this fact, here we skip the mathematic proof, but give an intuitive example in Fig.2.

<img src="https://www.dropbox.com/s/4v1q7bsmc0hn3hq/Screenshot%20from%202016-09-11%2019-05-41.png?dl=1" alt="Drawing" style="height: 400px;"/>

*Fig.2 pointwise supremum of the set of affine functions is convex function*

Fig.2 shows a set of affine functions, the red lines stands for the pointwise supremum. It is obvious that in this figure the red line function is a convex function. We know for convex function and set, it's easy to solve the global extreme. No matter $f(x)$ is convex or not, its dual function is concave, which is a convenient property.

## 4. Connection with conjugate function

When the constrains in Eq.(1) are linear, the dual function looks often similar to the conjugate function. In the following parts, I will try explain the connection between conjugate function and lagrange dual functions with my intuitive understanding.

#### Conjugate function

The conjugate function of $f(x)$ is defined as
{%math%}
\begin{equation}
f^{\star}(\lambda) = \sup_x(\lambda^T x- f(x))
\end{equation}
{%endmath%}

Here is an example about conjugate function.

<img src="https://www.dropbox.com/s/deed6i394rb10wy/Screenshot%20from%202016-09-12%2001-55-44.png?dl=1" alt="Drawing" style="height: 400px;"/>

*Fig.3 Conjugate function.*

As Fig.3 illustrated, the green curve is $f(x)$, the black solid line is $\lambda^T x$, the vectors (with sign) between $\lambda^T x$ and $f(x)$ are $\lambda^T x- f(x)$. So the conjugate function $f^{\star}(\lambda)$ is about the biggest gap between line $\lambda^T x$ and $f(x)$. From Fig.3 we can see, intuitively conjugate function describes lines with slope and intercept, which tangent to $f(x)$.

In this example, the maximum of $\lambda^T x- f(x)$ is the intercept of tangent line of $f(x)$ with slope $\lambda$. More generally, for convex function, the meaning of conjugate function is, given a slope $\lambda$, if $\sup_x(\lambda^T x- f(x))$ exists, then $g(\lambda)$ is the intercept of the tangent line of $f(x)$ with slope $\lambda$.

Another intuitive thinking is, if f(x) is "nicely" convex, when conjugate function $f^{\star}(\lambda)$ is given, which means all the tangent lines of $f(x)$ are given, the intersection of their epigraph is exactly the epigraph of $f(x)$, which means $f(x)$ is known too.

So conjugate function $f^{\star}(\lambda)$ is a function about tangent lines, which approximate of $f(x)$ with tangent lines. Conjugate function is another view of f(x).

#### When constrains are linear

When constrains are linear, our optimization problem looks like
$$
\min_x f(x) \quad s.t. \space Ax \preceq b
$$

The Lagrange dual function is
{% math %}
\begin{aligned}
g(\lambda) & = \inf_x (f(x)+\lambda^T(Ax-b)) && with \space \lambda \succeq 0\\
& = -\sup_x (-\lambda^T A x - f(x)) - \lambda^T b && \\
& = -f^{\star}(-\lambda^T A) - \lambda^T b && \text{due to Eq.9}
\end{aligned}
{% endmath %}

Intuitively, since $-\lambda^T b$ is a linear term, the extreme values only are effected by the domain, so if we focus on the $-f^{\star}(-\lambda^T A)$ term, to solve $\max_{\lambda} g(\lambda)$ means to solve $\min f^{\star}(-\lambda^T A)$.

What is the minimum of a conjugate function? again, let's see an example.

if the optimization problem is
$$
\min x^2 \quad s.t. x \leq 1
$$

we have the dual function as
{%math%}
\begin{align}
g(\lambda) &= -f^{\star}(-\lambda^T A) - \lambda^T b \\
&= -f^{\star}(-\lambda) - \lambda
\end{align}
{% endmath %}

here the conjugate function is
{% math %}
f^{\star}(\lambda)=\sup_x(\lambda x - f(x)) \\
= \frac{1}{4} \lambda^2
{% endmath %}

The dual problem is defined as
{%math%}
\begin{equation}
\max_{\lambda} g(\lambda) \\
\text{dom } g = \{-\lambda \in \text{dom } f^{\star}, \lambda \geq 0\}
\end{equation}
{%endmath%}

if we plug Eq.11 into Eq.12, we have dual problem like
$$
\max_\lambda -f^{\star}(-\lambda)-\lambda
$$

As we discussed before, $-\lambda$ is the linear term, the extreme value is only effected by its domain. Now let's consider the extreme of  $-f^{\star}(-\lambda)$. The problem is equivalent to solve

$$
\min_\lambda f^{\star}(-\lambda)
$$

In Fig.4, we can see, it is obvious when $\lambda = 0$, the conjugate function $f^{\star}(-\lambda)$ is minimized.

<img src="https://www.dropbox.com/s/ywwiilpfec84xjq/conjugate_function.gif?dl=1" alt="Drawing" style="height: 400px;"/>

*Fi.4 minimum of conjugate function. The same as Fig.1, $f(x)$ and $f^{\star}(\lambda)$ have independent variables, for a more concise expression, the orange curve should be plotted on an additional figure.*

Now lets go back to our questions, conjugate function is another way to describe the original function with support tangent lines. For convex function, the minimum of original function often means the tangent line in conjugate function, which has minimum intercept.
*note the intercept has sign here.*

When our optimization problem has linear constrains, the dual function is similar to the form of conjugate function, intuitively solve the dual problem is more likely to search the support tangent line on the extreme point.

### References

1. "Convex Optimization", [The Book][5], you can find every thing you need
2. A helpful discuss [thread][3]
3. [Proof][6] about conjugate function's properties, in chinese

### Copyright

© Zhiliang Zhou and https://masszhou.github.io, 2016. Unauthorized use and/or duplication of this material without express and written permission from this site’s author and/or owner is strictly prohibited. Excerpts and links may be used, provided that full and clear credit is given to Zhiliang Zhou and https://masszhou.github.io with appropriate and specific direction to the original content.

[1]:https://www.dropbox.com/s/0qbij8ef1teu1sg/Screenshot%20from%202016-09-11%2016-18-41.png?dl=1
[2]:https://www.dropbox.com/s/4v1q7bsmc0hn3hq/Screenshot%20from%202016-09-11%2019-05-41.png?dl=1
[3]:http://math.stackexchange.com/questions/223235/please-explain-the-intuition-behind-the-dual-problem-in-optimization
[4]:https://www.dropbox.com/s/deed6i394rb10wy/Screenshot%20from%202016-09-12%2001-55-44.png?dl=1
[5]:http://stanford.edu/~boyd/cvxbook/
[6]:http://www.cnblogs.com/murongxixi/p/3617054.html
[7]:https://www.dropbox.com/s/ywwiilpfec84xjq/conjugate_function.gif?dl=1

---
layout: post
title:  "Introduction to ADMM"
author: Snowkylin
categories: ADMM optimization
---

<script type="text/x-mathjax-config">

</script>

* TOC
{:toc}

# Duel Ascent

Consider the equality-constrained convex optimization problem:

<span markdown="0">
\begin{align}
\begin{split}
    \min & f(x) \\
    s.t. & Ax = b
\end{split}
\end{align}
</span>

Lagrangian function[^1]

<span markdown="0">
\begin{equation}
    L(x, y) = f(x) + y^T(Ax - b)
\end{equation}
</span>

Dual function[^2]

<span markdown="0">
\begin{align}
    g(y) = \min_x L(x, y) &= \min_x (f(x) + y^T Ax - y^T b) \\
    &= \min_x (f(x) + y^T Ax) - y^T b
\end{align}
</span>

Dual problem:

<span markdown="0">
\begin{align}
    \max_y & g(y) \\
\end{align}
</span>

So our task is to find the maximum of $g(y)$. In dual ascent method, we use gradient ascend to achieve this. That is:

<span markdown="0">
\begin{align}
    y^{k+1} \leftarrow y^k + \alpha \nabla g(y^k)
\end{align}
</span>

Then we need to get

<span markdown="0">
\begin{align}
    \nabla g(y) &= \frac{\partial}{\partial y} [\min_x (f(x) + y^T Ax) - y^T b] \\
    &= \frac{\partial}{\partial y} \min_x (f(x) + y^T Ax) - b
\end{align}
</span>

Assume that we already have $x^*$ (given $y$) as:

<span markdown="0">
\begin{align}
    x^* \leftarrow \arg \min_x (f(x) + y^T Ax)
\end{align}
</span>

Then we can calculate $g(y)$ as:

<span markdown="0">
\begin{align}
    \nabla g(y) &= \frac{\partial}{\partial y} \min_x (f(x) + y^T Ax) - b \\
    &= \frac{\partial}{\partial y} (f(x^*) + y^T Ax^*) - b \\
    &= Ax^* - b
\end{align}
</span>

So now we can update $y$ as:

<span markdown="0">
\begin{align}
    y^{k+1} &\leftarrow y^k + \alpha \nabla g(y^k) \\
    &= y^k + \alpha (Ax^* - b)
\end{align}
</span>

And this is the dual ascend method:

<span markdown="0">
\begin{align}
    x^* &\leftarrow \arg \min_x (f(x) + (y^k)^T Ax) = \arg \min_x L(x, y^k) \\
    y^{k+1} &\leftarrow y^k + \alpha (Ax^* - b)
\end{align}
</span>

# Method of Multipliers

The dual ascend method is not very robust in practice. $\min_x L(x, y^k)$ can fail when $L$ is unbounded for $x$. e.g., $f(x) = ax + b$. Then we want to add robustness to the method, without assumption of strictly convexity of $f$. Therefore, we consider the *augmented Lagrangian* as:

<span markdown="0">
\begin{align}
    L(x, y) &= f(x) + y^T(Ax - b) + \frac{\rho}{2} \| Ax - b\|_2^2 \\
    &= (f(x) + \frac{\rho}{2} \| Ax - b\|_2^2) + y^T(Ax - b)
\end{align}
</span>

Which is equal to the following problem

<span markdown="0">
\begin{align}
    \min & f(x) + \frac{\rho}{2} \| Ax - b\|_2^2 \\
    s.t. & Ax = b
\end{align}
</span>

# Reference

* Boyd, Stephen. “Distributed Optimization and Statistical Learning via the Alternating Direction Method of Multipliers.” Foundations and Trends® in Machine Learning 3, no. 1 (2010): 1–122. <https://doi.org/10.1561/2200000016>.
* Nocedal, Jorge, and Stephen J. Wright. Numerical Optimization. 2nd ed. Springer Series in Operations Research. New York: Springer, 2006.

# Note

[^1]: See page 330 of (Nocedal and Wright 2006)
[^2]: See page 344 of (Nocedal and Wright 2006)






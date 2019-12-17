---
layout: post
title:  "Introduction to ADMM"
author: Snowkylin
categories: ADMM optimization
---

* TOC
{:toc}

Consider the equality-constrained convex optimization problem:

<span markdown="0">
\begin{align}
    \min & f(x) \\
    s.t. & Ax = b
\end{align}
</span>

# Duel Ascent

Lagrangian:

<span markdown="0">
\begin{equation}
    L(x, y) = f(x) + y^T(Ax - b)
\end{equation}
</span>

Dual function:

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

So our task is to find the maximum of $g(y)$. In dual ascent method, we use gradient ascend. That is:

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

Consider that we already have $x^*$ (given $y$) as:

<span markdown="0">
\begin{align}
    x^* \leftarrow \arg \min (f(x) + y^T Ax)
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
    &= y^k + \alpha (Ax^{*k+1} - b)
\end{align}
</span>

And this is dual ascend method:

<span markdown="0">
\begin{align}
    x^{*k+1} &\leftarrow \arg \min (f(x) + y^{kT} Ax) \\
    y^{k+1} &\leftarrow y^k + \alpha (Ax^{*k+1} - b)
\end{align}
</span>

# Augmented Lagrangians (Method of Multipliers)

To add robostness to the above



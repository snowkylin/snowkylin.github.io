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

## Duel Ascent

Consider the equality-constrained convex optimization problem:

$$
\begin{equation}
\begin{split}
    \min \; & f(x) \\
    s.t. \; & Ax = b 
\end{split}\label{eq:primal_problem}
\end{equation}
$$

Lagrangian function[^1]

$$
\begin{equation*}
    L(x, y) = f(x) + y^T(Ax - b)
\end{equation*}
$$

Dual function[^2]

$$
\begin{align*}
    g(y) = \min_x L(x, y) &= \min_x (f(x) + y^T Ax - y^T b) \\
    &= \min_x (f(x) + y^T Ax) - y^T b
\end{align*}
$$

Dual problem:

$$
\begin{align*}
    \max_y & g(y) \\
\end{align*}
$$

So our task is to find the maximum of $g(y)$. In dual ascent method, we use gradient ascend to achieve this. That is:

$$
\begin{align*}
    y^{k+1} \leftarrow y^k + \alpha \nabla g(y^k)
\end{align*}
$$

Then we need to get the gradient $\nabla g(y)$

$$
\begin{align*}
    \nabla g(y) &= \frac{\partial}{\partial y} [\min_x (f(x) + y^T Ax) - y^T b] \\
    &= \frac{\partial}{\partial y} \min_x (f(x) + y^T Ax) - b
\end{align*}
$$

Assume that we already have $x^*$ (given $y$) as:

$$
\begin{align*}
    x^* \leftarrow \arg \min_x (f(x) + y^T Ax)
\end{align*}
$$

Then we can calculate $g(y)$ as:

$$
\begin{align*}
    \nabla g(y) &= \frac{\partial}{\partial y} \min_x (f(x) + y^T Ax) - b \\
    &= \frac{\partial}{\partial y} (f(x^*) + y^T Ax^*) - b \\
    &= Ax^* - b
\end{align*}
$$

That means, if we have $$x^*$$ given, $\nabla g(y)$ is simply $Ax^* - b$. So now we can update $y$ as:

$$
\begin{align*}
    y^{k+1} &\leftarrow y^k + \alpha \nabla g(y^k) \\
    &= y^k + \alpha (Ax^* - b)
\end{align*}
$$

And this is the dual ascend method to solve $\eqref{eq:primal_problem}$

$$
\begin{align*}
    x^* &\leftarrow \arg \min_x (f(x) + (y^k)^T Ax) = \arg \min_x L(x, y^k) \\
    y^{k+1} &\leftarrow y^k + \alpha (Ax^* - b)
\end{align*}
$$

## Method of Multipliers

### Augmented Lagrangian

The dual ascend method is not very robust in practice. $\min_x L(x, y^k)$ can fail when $L$ is unbounded for $x$. e.g., $f(x) = ax + b$. Then we want to add robustness to the method, without assumption of strictly convexity of $f$. Therefore, we consider the *augmented Lagrangian* as:

$$
\begin{align*}
    L_\rho(x, y) &= f(x) + y^T(Ax - b) + \frac{\rho}{2} \| Ax - b\|_2^2 \\
    &= (f(x) + \frac{\rho}{2} \| Ax - b\|_2^2) + y^T(Ax - b)
\end{align*}
$$

in which $\rho > 0$ is the penalty parameter. Notice that $$\frac{\rho}{2} \| Ax - b\|_2^2$$ is a typical strictly convex function. Therefore, considering $f$ is convex, $$f(x) + \frac{\rho}{2} \| Ax - b\|_2^2$$ is also strictly convex. The larger $\rho$ is, the more convex the Lagrangian function will be. 

The above augmented Lagrangian can be derived from the following problem

$$
\begin{align}
\begin{split}
    \min \; & f(x) + \frac{\rho}{2} \| Ax - b\|_2^2 \\
    s.t. \; & Ax = b
\end{split}\label{eq:modified_primal_problem}
\end{align}
$$

This problem is just equaivalent to $\eqref{eq:primal_problem}$, since for all feasible $x$, $Ax = b$, then the added factor $$\frac{\rho}{2} \| Ax - b\|_2^2$$ will be zero.

### Selection of step size $\alpha$

The next question is the selection of the step size $\alpha$. After getting $$x^* = \arg \min_x L_\rho(x, y^k)$$, we hope to make a better selection of $\alpha$ so that given $y^k$ and $$x^*$$, we can calculate $$y^{k+1} \leftarrow y^k + \alpha^* (Ax^* - b)$$ which makes $$x^* = \arg \min_x L(x, y^{k+1})$$ given $y^{k+1}$. Which means, if we suddenly switch the problem from $\eqref{eq:modified_primal_problem}$ to $\eqref{eq:primal_problem}$ after getting $$x^*$$ and $y^{k+1}$ in step $k$, we do not need to calculate $x$ again since $$x^*$$ in the former step $k$ is already the minimum of $L(x, y^{k+1})$. The tricky part is, by setting $\alpha^* = \rho$, we can achieve this goal. To see this, let's assume $$x^* = \arg \min_x L(x, y^{k+1})$$ when $$\alpha = \alpha^*$$, then

$$
\begin{align}
    \frac{\partial L(x, y^{k+1})}{\partial x}\rvert_{x = x^*} &= 0 \nonumber \\
    \frac{\partial}{\partial x}(f(x) + (y^{k+1})^T Ax - (y^{k+1})^T b)\rvert_{x = x^*} &= 0 \nonumber \\
    \nabla_x f(x^*) + A^T(y^k + \alpha^*(Ax^* - b)) &= 0 \label{eq:1.3}
\end{align}
$$

That means, if we can find $$\alpha^*$$ that satisfy $\eqref{eq:1.3}$ then we can achieve our goal.

We already know that $$x^* = \arg \min_x L_\rho(x, y^k)$$, so

$$
\begin{align}
    \frac{\partial L_\rho(x, y^k)}{\partial x}\rvert_{x = x^*} &= 0 \nonumber \\
    \frac{\partial}{\partial x}(f(x) + (y^k)^T(Ax - b) + \frac{\rho}{2} \| Ax - b\|_2^2)\rvert_{x = x^*} &= 0 \nonumber \\
    \nabla_x f(x^*) + A^Ty^k + \rho A^T(Ax^* - b) &= 0 \label{eq:1.4}
\end{align}
$$

By $\eqref{eq:1.4}$ we can find that if we take $$\alpha^* = \rho$$, then $\eqref{eq:1.3}$ is true.

If we apply dual ascend method to our modified problem $\eqref{eq:modified_primal_problem}$ (so the Lagrangian function is $L_\rho$) and set step size $\alpha = \rho$, we get the method of multipliers to solve $\eqref{eq:primal_problem}$:

$$
\begin{align*}
    x^* &\leftarrow \arg \min_x L_\rho(x, y^k) \\
    y^{k+1} &\leftarrow y^k + \rho (Ax^* - b)
\end{align*}
$$



## Reference

* Boyd, Stephen. “Distributed Optimization and Statistical Learning via the Alternating Direction Method of Multipliers.” Foundations and Trends® in Machine Learning 3, no. 1 (2010): 1–122. <https://doi.org/10.1561/2200000016>.
* Nocedal, Jorge, and Stephen J. Wright. Numerical Optimization. 2nd ed. Springer Series in Operations Research. New York: Springer, 2006.

## Note

[^1]: See page 330 of (Nocedal and Wright 2006)
[^2]: See page 344 of (Nocedal and Wright 2006)






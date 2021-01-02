---
layout: post
title:  "Introduction to Simplex and Interior-Point Methods to Solve Linear Programming"
author: Snowkylin
categories: LP Simplex Interior-Point optimization
---

<script type="text/x-mathjax-config">

</script>

<style>
table {
    border: 0px;
}
table td {
    border: 0px;
}
</style>

* TOC
{:toc}

## Problem Definition

Consider the standard form of linear programming

\begin{equation}
    \min \; c^T x \; \text{subject to} \; Ax = b, x \geq 0
    \label{lp}
\end{equation}

in which $c, x \in \mathbb{R}^n$, $b \in \mathbb{R}^m$, $A \in \mathbb{R}^{m \times n}$. ($n$ is the number of variables, $m$ is the number of constraints)

The Lagrangian function is

\begin{equation}
    \mathcal{L}(x, \lambda, s) = c^T x - \lambda^T(Ax - b) - s^T x
\end{equation}

in which $\lambda \in \mathbb{R}^m$ (lagrange multiplier for equality constraint $Ax = b$), $s \in \mathbb{R}^n$ (lagrange multiplier for inequality constraint $x \geq 0$).

The KKT condition is

$$
\begin{align}
    \frac{\partial \mathcal{L}}{\partial x} = c - A^T \lambda - s = 0 \rightarrow \; & A^T \lambda + s = c \label{KKT1}\\
    & Ax = b \label{KKT2}\\
    & x \geq 0 \label{KKT3} \\
    \text{inequality lagrange multiplier} \geq 0 \rightarrow \; & s \geq 0 \label{KKT4} \\
    \text{complementarity condition for inequality} \rightarrow \; & x_i s_i = 0 \label{KKT5}
\end{align}
$$

## The Simplex Method

Assume that the $m \times n$ matrix $A$ has full row rank, we select $m$ columns of $A$ to form a nonsingular $m \times m$ square matrix $B$ and denote the index set as $\mathcal{B}$ (and the remaining columns as a $m \times (n - m)$ matrix $N$, and corresponding index set $\mathcal{N} = \{1, \cdots, n\} \backslash \mathcal{B}$). The n-dimensional vectors $x, s$ and $c$ are also partitioned into $m$-dimensional vectors $x_B, s_B, c_B$ and $(n - m)$-dimensional vectors $x_N, s_N, c_N$.

Here $\mathcal{B}$ is called a _basis_, and a _basic feasible point_ is a feasible point satisfying \eqref{KKT2}, \eqref{KKT3} and $x_N = 0$. From \eqref{KKT2} and the partition, we know that $Ax = (B, N)\begin{pmatrix}x_B\\\\x_N\end{pmatrix} = Bx_B + Nx_N = b$ [^1] . Since $x_N = 0$ and $B$ is nonsingular (invertible), we know $x_B = B^{-1}b$.

Assume $x_B = B^{-1}b \geq 0$ (we only deal with basic feasible points), To satisfy the complementary condition \eqref{KKT5}, we need to set $s_B = 0$. Then \eqref{KKT1} can be partitioned as

\begin{equation}
    \begin{pmatrix}B^T\\\\N^T\end{pmatrix}\lambda + \begin{pmatrix}s_B\\\\s_N\end{pmatrix} = \begin{pmatrix}c_B\\\\c_N\end{pmatrix}
\end{equation}

Then we can calculate $\lambda$ and $s_N$ as

$$
\begin{align}
    B^T\lambda &= c_B \Rightarrow \lambda = (B^T)^{-1}c_B\\
    N^T\lambda + s_N &= c_N \Rightarrow s_N = c_N - N^T\lambda = c_N - N^T(B^T)^{-1}c_B \label{pricing}
\end{align}
$$

The computation of $s_N$ \eqref{pricing} is usually called as _pricing_.

Now all the value of variables $x (x_B, x_N), \lambda$ and $s (s_B, s_N)$ is known given basis $\mathcal{B}$, concluded below

$$
\begin{align*}
    x_B &= B^{-1}b, x_N = 0\\
    \lambda &= (B^T)^{-1}c_B\\
    s_B &= 0, s_N = c_N - (B^{-1}N)^T c_B
\end{align*}
$$

By the process of value selection above, our $x, \lambda$ and $s$ satisfy \eqref{KKT1}, \eqref{KKT2} and \eqref{KKT5} (\eqref{KKT3} ($x \geq 0$) is also satisfied since we only deal with basic feasible points). Then the only condition that may not be satisfied is \eqref{KKT4} ($s \geq 0$). In fact, if $s_N$ calculated in \eqref{pricing} satisfy $s_N \geq 0$, then all KKT conditions are satisfied and we get the optimal solution.

However, there may exist some index $q \in \mathcal{N}$ for which $s_q < 0$ (thus \eqref{KKT4} is not satisfied. Notice that $s_B = 0$ so $q$ must be in $\mathcal{N}$), so our current $(x, \lambda, s)$ is still not optimal. A good news is that, by increasing the value of $x_q$ from 0 to certain value $x_q^+$ ($x_B$ will also change to satisfy $Ax = b$, until one of the component of $x_B$ is driven to zero), then the objective will decrease (proof is in (Nocedal and Wright 2006, p. 369)). 

Denote the next iteration of $x$ as $x^+$. Since both of them need to satisfy the equality constraint ($Ax = b$ and $Ax^+ = b$), we have

\begin{equation}
    Ax^+ = (B, N)\begin{pmatrix}x^+_B\\\\0\\\\\vdots\\\\x^+_q\\\\\vdots\\\\0\end{pmatrix} = Bx^+_B + A_q x^+_q = Bx_B = Ax
\end{equation}

so

\begin{equation}
    x^+_B = x_B - B^{-1}A_q x^+_q\label{x_B_update}
\end{equation}

Then we want to find the maximum $x^+_q$ so that $x^+_B \geq 0$. Denote $d = B^{-1}A_q$ (that is, the solution of $Bd = A_q$), then

$$
\begin{align*}
    \forall i \in \mathcal{B}, x_i - d_i x^+_q \geq 0\\
    x^+_q \leq \frac{x_i}{d_i} (d_i > 0)
\end{align*}
$$

then $x^+_q = \min\_{i \rvert i \in \mathcal{B}, d_i > 0} \frac{x_i}{d_i}$ (use $p$ to denote the minimizing $i$. That is, the component of $x_B$ which is driven to zero). With $x^+_q$ known, we can update $x^+_B$ by \eqref{x_B_update} and update $x^+_N$ by $(0, \cdots, x^+_q, \cdots, 0)$. $\mathcal{B}$ is also updated by adding $q$ and removing $p$.

<script src="https://emgithub.com/embed.js?target=https%3A%2F%2Fgithub.com%2Fsnowkylin%2Flp-algorithm%2Fblob%2Fmaster%2Fsimplex.py&style=github&showBorder=on&showLineNumbers=on&showFileMeta=on"></script>

## Interior-Point Method

Another way to optimize \eqref{lp}) is to just solve the KKT condition directly. That is, to find a solution that satisfies the three equality conditions \eqref{KKT1}\eqref{KKT2}\eqref{KKT5} while maintaining $(x, s) \geq 0$ (\eqref{KKT3} and \eqref{KKT4}. This is strictly satisfied in the whole solving process so it is called "interior-point" method). i.e., solve

\begin{equation}
    \begin{pmatrix}A^T\lambda + s - c\\\\Ax - b\\\\x_i s_i\end{pmatrix} = 0
\end{equation}

Define $F(x, \lambda, s) = \begin{pmatrix}A^T\lambda + s - c\\\\Ax - b\\\\x_i s_i\end{pmatrix} = \begin{pmatrix}A^T\lambda + s - c\\\\Ax - b\\\\XSe\end{pmatrix}$ (from $\mathbb{R}^{n+m+n}$ to $\mathbb{R}^{n+m+n}$, $X = \text{diag}(x)$, $S = \text{diag}(s)$, $e = (1, \cdots, 1)^T$), then we need to solve the equation $F(x, \lambda, s) = 0$. 

We can use Newton's method to solve this equation (details can be found in Chapter 11 of (Nocedal and Wright 2006)). Given current iterate $z^k = (x^k, \lambda^k, s^k)$, we linearize the function $F$ on point $z^k$ as $m(p) = F(z^k + p) \approx F(z^k) + \nabla F(z^k)^T p$ and solve the approximated equation $m(p) = 0$ to find the step $p$. A tricky part is to different the function $F$ whose input and output are both multi-dimensional vectors (let's say, $\mathbb{R}^n \rightarrow \mathbb{R}^m$). Actually, this is equivalent to different respecting to each component of the output separately (then we have $m$ $\mathbb{R}^n$ column vectors), and concatenate these vectors as a $n \times m$ matrix. More specifically,

\begin{equation}
    \nabla F(x) = (\nabla F_1(x), \cdots, \nabla F_m(x)) = \begin{pmatrix}
    \frac{\partial F_1}{\partial x_1} & \cdots & \frac{\partial F_m}{\partial x_1} \\\\\\
    \vdots && \vdots \\\\\\
    \frac{\partial F_1}{\partial x_n} & \cdots & \frac{\partial F_m}{\partial x_n}
    \end{pmatrix}
\end{equation}

Often, for notational convenience, we prefer to work with the transpose of the matrix (see (Nocedal and Wright 2006, p. 627), that is, $J(x) = \nabla F(x)^T$, which is called \textit{Jacobian matrix}.

Then the equation $m(p) = 0$ ($\nabla F(z^k)^T p = -F(z^k)$) becomes

$$
\begin{align}
    \begin{pmatrix}
        \frac{\partial A^T\lambda + s - c}{\partial x} & \frac{\partial Ax - b}{\partial x} & \frac{\partial XSe}{\partial x} \\
        \frac{\partial A^T\lambda + s - c}{\partial \lambda} & \frac{\partial Ax - b}{\partial \lambda} & \frac{\partial XSe}{\partial \lambda} \\
        \frac{\partial A^T\lambda + s - c}{\partial s} & \frac{\partial Ax - b}{\partial s} & \frac{\partial XSe}{\partial s}
    \end{pmatrix}^T p &= -
    \begin{pmatrix}
        A^T\lambda + s - c\\Ax - b\\XSe
    \end{pmatrix}\nonumber\\
    \begin{pmatrix}
        0 & A^T & S \\
        A & 0 & 0 \\
        I & 0 & X
    \end{pmatrix}^T p &= -
    \begin{pmatrix}
        A^T\lambda + s - c\\Ax - b\\XSe
    \end{pmatrix}\nonumber\\
    \begin{pmatrix}
        0 & A & I \\
        A^T & 0 & 0 \\
        S & 0 & X
    \end{pmatrix} p &= -
    \begin{pmatrix}
        A^T\lambda + s - c\\Ax - b\\XSe
    \end{pmatrix}
\end{align}
$$

Solve the equation and we get the (raw) direction $p = (\Delta x, \Delta \lambda, \Delta s)$. 

The next step is to find the step length $\alpha$ so that $(x^{k+1}, \lambda^{k+1}, s^{k+1}) = (x^k, \lambda^k, s^k) + \alpha (\Delta x, \Delta \lambda, \Delta s)$. A simple strategy is to find the largest $\alpha$ so that $(x, s) \geq 0$. Thus

$$
\begin{align*}
    \forall i = 1 \cdots n, x^k_i + \alpha^{\text{pri}}\Delta x_i \geq 0 &\rightarrow \forall i = 1 \cdots n \text{ and } \Delta x_i < 0, \alpha^{\text{pri}} \leq -\frac{x^k_i}{\Delta x_i}\\
    \forall i = 1 \cdots m, s^k_i + \alpha^{\text{dual}}\Delta s_i \geq 0 &\rightarrow \forall i = 1 \cdots n \text{ and } \Delta s_i < 0, \alpha^{\text{dual}} \leq -\frac{s^k_i}{\Delta s_i}
\end{align*}
$$

So $\alpha^{\text{pri}} = \min_{i: \Delta x_i < 0}-\frac{x^k_i}{\Delta x_i}$ and $\alpha^{\text{dual}} = \min_{i: \Delta s_i < 0}-\frac{s^k_i}{\Delta s_i}$. Practical step length will be slightly smaller than this (see (Nocedal and Wright 2006, p. 409)).

<script src="https://emgithub.com/embed.js?target=https%3A%2F%2Fgithub.com%2Fsnowkylin%2Flp-algorithm%2Fblob%2Fmaster%2Finterior_point.py&style=github&showBorder=on&showLineNumbers=on&showFileMeta=on"></script>

## Reference

* Nocedal, Jorge, and Stephen J. Wright. _Numerical Optimization_. 2nd ed. Springer Series in Operations Research. New York: Springer, 2006.

## Note

[^1]: Here we assume $\mathcal{B}$ is the first $m$ indices for easier understanding, which is not necessary.
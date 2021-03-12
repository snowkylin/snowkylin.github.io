---
layout: post
title:  "Implicit Differentiation Note"
author: Snowkylin
categories: math
---

Assume we know $\frac{\partial L}{\partial y}$ and $y = a_1 x_1 + a_2 x_2$, then we have

$$
\begin{align*}
\frac{\partial L}{\partial x_1} &= \frac{\partial L}{\partial y}\frac{\partial y}{\partial x_1} = a_1\frac{\partial L}{\partial y} \\
\frac{\partial L}{\partial x_2} &= \frac{\partial L}{\partial y}\frac{\partial y}{\partial x_2} = a_2\frac{\partial L}{\partial y}
\end{align*}
$$

However, what if we know $\frac{\partial L}{\partial y_1}$, $\frac{\partial L}{\partial y_2}$ and $x = a_1 y_1 + a_2 y_2$? In this case, we have

$$
\frac{\partial L}{\partial x} = \frac{\partial L}{\partial y_1}\frac{\partial y_1}{\partial x} + \frac{\partial L}{\partial y_2}\frac{\partial y_2}{\partial x}
$$

but $\frac{\partial y_1}{\partial x}$ and $\frac{\partial y_2}{\partial x}$ are hard to tackle since we cannot explicitly write $y_1$ and $y_2$ as functions of $x$ ($y_1 = f(x)$ and $y_2 = g(x)$) with only one equation $x = a_1 y_1 + a_2 y_2$ (we can solve it if we have two). However, we know that $f(x)$ and $g(x)$ are dependent: assume we know the explicit form of $y_1 = f(x)$ (here $f(x)$ is totally free and can be any function of $x$, e.g., $f(x) = x^2, f(x) = e^x, \cdots$), then we immediately have the explicit form of $y_2 = g(x) = \frac{x}{a_2} - \frac{a_1}{a_2}f(x)$, and $g'(x) = \frac{1}{a_2} - \frac{a_1}{a_2}f'(x)$. Or we can reformulate it as

$$
\begin{equation}
a_1 f'(x) + a_2 g'(x) = 1 \label{eq1}
\end{equation}
$$

Note that $\frac{\partial L}{\partial x}$ is not unique since $f(x)$ (and thus $f'(x)$) can be in arbitrary form. E.g., $f(x) = \frac{1}{2 a_1} x, g(x) = \frac{1}{2 a_2} x$ and $f(x) = 0, g(x) = \frac{1}{a_2} x$ are both valid functions satisfying $x = a_1 y_1 + a_2 y_2$ (you can also check that they both satisfy $\eqref{eq1}$). And the value of $\frac{\partial L}{\partial x}$ are $\frac{1}{2 a_1}\frac{\partial L}{\partial y_1} + \frac{1}{2 a_2}\frac{\partial L}{\partial y_2}$ and $\frac{1}{a_2}\frac{\partial L}{\partial y_2}$ respectively.

Alternatively, we have an easier way to obtain $\eqref{eq1}$ by differencing $x$ at both side of $x = a_1 y_1 + a_2 y_2$.

Now assume we have two equations

$$
\begin{align}
x_1 &= a_1 y_1 + a_2 y_2 \label{eq2}\\
x_2 &= a_3 y_1 + a_4 y_2 \label{eq3}
\end{align}
$$

and similarly we have $y_1 = f(x_1, x_2), y_2 = g(x_1, x_2)$. Then $y_2 = g(x_1, x_2) = \frac{x_1}{a_2} - \frac{a_1}{a_2}f(x_1, x_2)$ (via $\eqref{eq2}$) or $\frac{x_2}{a_4} - \frac{a_3}{a_4}f(x_1, x_2)$ (via $\eqref{eq3}$). We take the partial differentiation of $g(x_1, x_2)$ for $x_1$ and $x_2$, then

$$
\begin{align*}
\frac{\partial g(x_1, x_2)}{\partial x_1} &= \frac{1}{a_2} - \frac{a_1}{a_2}\frac{\partial f(x_1, x_2)}{\partial x_1} (\text{via }\eqref{eq2}) \\ \text{ or } &= - \frac{a_3}{a_4}\frac{\partial f(x_1, x_2)}{\partial x_1} \text{ via $\eqref{eq3}$, note that $\frac{x_2}{a_4}$ vanished since $\frac{\partial x_2}{\partial x_1} = 0$} \\
\frac{\partial g(x_1, x_2)}{\partial x_2} &= - \frac{a_1}{a_2}\frac{\partial f(x_1, x_2)}{\partial x_2} \text{ via $\eqref{eq2}$, note that $\frac{x_1}{a_2}$ vanished since $\frac{\partial x_1}{\partial x_2} = 0$} \\ \text{ or } &= \frac{1}{a_4} - \frac{a_3}{a_4}\frac{\partial f(x_1, x_2)}{\partial x_2} (\text{via }\eqref{eq3}) \\
\end{align*}
$$

We can rearrange the above equations as

$$
\begin{align*}
a_1\frac{\partial f(x_1, x_2)}{\partial x_1} + a_2\frac{\partial g(x_1, x_2)}{\partial x_1} &= 1 \\
a_3\frac{\partial f(x_1, x_2)}{\partial x_1} + a_4\frac{\partial g(x_1, x_2)}{\partial x_1} &= 0 \\
a_1\frac{\partial f(x_1, x_2)}{\partial x_2} + a_2\frac{\partial g(x_1, x_2)}{\partial x_2} &= 0 \\
a_3\frac{\partial f(x_1, x_2)}{\partial x_2} + a_4\frac{\partial g(x_1, x_2)}{\partial x_2} &= 1 
\end{align*}
$$

or, more compactly

$$
\begin{pmatrix}a_1 & a_2\\a_3 & a_4\end{pmatrix}
\begin{pmatrix}\frac{\partial y_1}{\partial x_1} & \frac{\partial y_1}{\partial x_2}\\\frac{\partial y_2}{\partial x_1} & \frac{\partial y_2}{\partial x_2}\end{pmatrix} = \begin{pmatrix}1 & 0\\0 & 1\end{pmatrix}
$$

We can also obtain the above equation by differencing $x_1$ and $x_2$ at both side of $\eqref{eq2}$ and $\eqref{eq3}$.

Extending this to $x = Ay$ in which $x$ and $y$ are both vectors and $A$ is a $M \times N$ matrix. And we have a implicit function $y = y(x) = (y_1(x), \cdots, y_N(x))$. Note that 

$$
\frac{\partial y}{\partial x} = 
\begin{pmatrix}
\frac{\partial y_1}{\partial x_1} & \cdots & \frac{\partial y_1}{\partial x_M}\\
\vdots & \ddots & \vdots \\
\frac{\partial y_N}{\partial x_1} & \cdots & \frac{\partial y_N}{\partial x_M}
\end{pmatrix}
$$

is the Jacobian matrix. So the equation can be written as

$$
A\frac{\partial y}{\partial x} = I
$$

(it can also be obtained by differencing $x$ at both side of $x = Ay$)

and

$$
\frac{\partial L}{\partial x} = \frac{\partial L}{\partial y_1}\frac{\partial y_1}{\partial x} + \cdots + \frac{\partial L}{\partial y_N}\frac{\partial y_N}{\partial x} = (\frac{\partial y}{\partial x})^T\begin{pmatrix}\frac{\partial L}{\partial y_1} \\ \vdots \\ \frac{\partial L}{\partial y_N}\end{pmatrix}
$$

If $A$ is an invertible square matrix, then we have $\frac{\partial y}{\partial x} = A^{-1}$, and 

$$
\frac{\partial L}{\partial x} = (A^T)^{-1}\begin{pmatrix}\frac{\partial L}{\partial y_1} \\ \vdots \\ \frac{\partial L}{\partial y_N}\end{pmatrix}
$$

(Note that $(A^{-1})^T = (A^T)^{-1}$)

## References

- Amos, B., & Kolter, J. Z. (2017). OptNet: Differentiable Optimization as a Layer in Neural Networks. International Conference on Machine Learning, 136–145. <http://proceedings.mlr.press/v70/amos17a.html>
- <https://tutorial.math.lamar.edu/classes/calci/implicitdiff.aspx>
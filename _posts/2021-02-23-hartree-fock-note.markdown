---
layout: post
title:  "Hartree-Fock Theory Note"
author: Snowkylin
categories: quantum ab-initio
---

* TOC
{:toc}

## Problem statement

We want to solve the following time-independent Schrodinger equation:

$$
\begin{equation}
[\underbrace{-\frac{1}{2}\sum_i \nabla_i^2}_{T_e(r)} + \underbrace{\sum_{A,i}-\frac{Z_A}{r_{Ai}}}_{V_{eN}(r, R)} + \underbrace{\sum_{i < j}\frac{1}{r_{ij}}}_{V_{ee}(r)}]\Psi(r_1, \cdots, r_N) = E\Psi(r_1, \cdots, r_N) \label{eq1}
\end{equation}
$$

in which $\nabla_t^2 = \frac{\partial^2}{\partial r_{i, x}^2} + \frac{\partial^2}{\partial r_{i, y}^2} + \frac{\partial^2}{\partial r_{i, z}^2}$ is the laplace operator for $r_i = (r_{i, x}, r_{i, y}, r_{i, z})$, $N$ is the number of electrons. $r_i, r_j$ and $R_A, R_B$ are the 3d coodinate of electrons $\_i, \_j$ (电子) and nuciels $\_A, \_B$ (原子核) respectively. $r_{iA} = \|r_i - R_A\|$ is the distance between electron $i$ and nuciel $A$, $r_{ij} = \|r_i - r_j\|$ is the distance between electron $i$ and $j$. 

- $T_e(r)$: kinetic energy of electrons
- $V_{eN}(r, R)$: electrostatic potential (静电势) between electrons ($_e$) and nuciels ($_N$) (or "external potentials")
- $V_{ee}(r)$: electrostatic repulsion (静电排斥) between electrons

The solution of the equation is a "wave function" $\Psi(r_1, \cdots, r_N)$ so that the equation always holds for any $r_1, \cdots, r_N$. Additionally, $\|\Psi(r_1, \cdots, r_N)\|^2$ is the probability that electron 1 appear at coodinate $r_1$ and electron 2 appears at coodinate $r_2$ and so on.

Here we assume that the nuciels are fixed so all $R$ are constants (the Born-Oppenheimer approximation). Therefore we do not consider $T_N(R)$ (kinetic energy of nucleis) and $V_{NN}(R)$ (Coulombic repulsion between nucleis) which are also constants.

Let $h(i) = -\frac{1}{2}\nabla_i^2 - \sum_{A}\frac{Z_A}{r_{Ai}}$, then the equation can be also written as

$$
[\sum_i h(i) + \sum_{i < j}\frac{1}{r_{ij}}]\Psi(r_1, \cdots, r_N) = E\Psi(r_1, \cdots, r_N)
$$

There are two challenges to solve this equation:

1. $V_{ee}(r) = \sum_{i < j}\frac{1}{r_{ij}}$ is hard to tackle, since it makes all electrons correlated. Without it, we can simply write the equation as $\sum_i h(i) \Psi(r_1, \cdots, r_N) = E\Psi(r_1, \cdots, r_N)$. In this case, we only need to solve $N$ single-electron equations $h(i)\psi_i(r_i) = \epsilon_i\psi_i(r_i)$ for $i = 1, \cdots, N$, get $N$ wave functions $\psi_1(r_1), \cdots, \psi_N(r_N)$ (molecular spatial orbitals (空间轨道). *Orbital* is a wave function for a single electron, see section 2.2 of [1]), then let $\Psi(r_1, \cdots, r_N) = \prod_i \psi_i(r_i)$ (hartree product), $E = \sum_i \epsilon_i$, and the equation is solved.
2. The wave function $\Psi(r_1, \cdots, r_N)$ needs to satisfy a specific physical property named *antisymmetry principle*, whose famous representation is *Pauli exclusion principle* (泡利不相容原理) that tell us two electrons in the same spatial orbital must have opposite spins. To illustrate this, we need to take spin $\omega = \\\{\alpha, \beta\\\}$ (自旋) into account. Let space-spin coordinates $x = (r, \omega)$ and change the molecular spatial orbital $\psi(r)$ to molecular spin orbital (自旋轨道) $\chi(x) = \psi(r)\omega$. Then the antisymmetry principle requires that $\Psi(\cdots, x_i, \cdots, x_j, \cdots) = -\Psi(\cdots, x_j, \cdots, x_i, \cdots)$. e.g., for a two-electron system, $\Psi(x_1, x_2) = -\Psi(x_2, x_1)$, and when $x_1 = x_2$ we have $\Psi(x_1, x_2) = -\Psi(x_1, x_2) \Rightarrow \Psi(x_1, x_2)  = 0 \Rightarrow \|\Psi(x_1, x_2)\|^2 = 0$, which means the probability of two electrons in the same space with parallel spin is zero.

The Hartree-Fock theory tackles these challenges by the following ways:

1. (Hartree Approximation) For the first challenge, assume $\Psi(r_1, \cdots, r_N) = \prod_i \psi_i(r_i)$ (hartree product, here we use an approximated wave function), then we minimize the total energy of $\eqref{eq1}$ using variational method, and find that: with the assumed wave function, for electron $i$, we can approximate its interaction with other electrons by the average field of other electrons, and we add all the average field to approximate $V_{ee}$. i.e., let $V_{ee} = \sum_i g(i)$. In this case we can let $h'(i) = h(i) + g(i)$ so we can write the equation as $\sum_i h'(i) \Psi(r_1, \cdots, r_N) = E\Psi(r_1, \cdots, r_N)$ and solve it easily as single-electron equations $h'(i)\psi_i(r_i) = \epsilon_i\psi_i(r_i)$ (not so easy though, since $g(i)$ includes the wave function of other electrons, and a self-consistent iteration is needed).
2. (Hartree-Fock Approximation) For the second challenge, assume $\Psi(x_1, \cdots, x_N) = \frac{1}{\sqrt{N!}}\begin{vmatrix}\chi_1(x_1) & \chi_2(x_1) & \cdots & \chi_N(x_1)\\\\ \chi_1(x_2) & \chi_2(x_2) & \cdots & \chi_N(x_2)\\\\ \vdots & \vdots & \ddots & \vdots\\\\ \chi_1(x_N) & \chi_2(x_N) & \cdots & \chi_N(x_N)\end{vmatrix}$ (slater determinant), which is a better approximation of the wave function that satisfy antisymmetry principle. E.g., for the two-electron case, $\Psi(x_1, x_2) = \frac{1}{\sqrt{2}}\begin{vmatrix}\chi_1(x_1) & \chi_2(x_1) \\\\ \chi_1(x_2) & \chi_2(x_2)\end{vmatrix} = \frac{1}{\sqrt{2}}[\chi_1(x_1)\chi_2(x_2) - \chi_2(x_1)\chi_1(x_2)]$. It is easy to check that $\Psi(x_1, x_2) = -\Psi(x_2, x_1)$, and if the two electrons are described by the same spin orbital $\chi = \chi_1 = \chi_2$, then $\Psi(x_1, x_2) = 0$.

## The simplest case (both $V_{ee}$ and antisymmetry principle ignored)

As mentioned before, if both $V_{ee}$ and antisymmetry principle are ignored, then we can just solve the following one-electron differential equation

$$
\begin{equation}
h(i)\psi_i(r_i) = \epsilon_i\psi_i(r_i) \label{eq2}
\end{equation}
$$

The solution of a differential equation is a function. That is, we want to find some functions $\psi_i(r_i)$ (molecular orbicals) so that the above equation holds for all $r_1 \in \mathbb{R}^3$.

However, such equation can be hard to solve in function space. Therefore we want to expand the wave function (molecular orbical) as the linear conbination of some (let's say, $K$) basis functions, $\psi_i(r_i) = \sum_{u=1}^K C_{ui} \phi_u(r_i)$. Then the problem of solving $\psi$ (in function space) transforms to solving $K$ coefficients $C_{1i}, \cdots, C_{Ki}$ (in vector space $\mathbb{R}^K$), which can be easier to solve with linear algebra.

For the selection of basis function, a common practice is to use known atomic orbital of each atoms (molecular orbitals $\psi_i$ are formed as a linear combination of atomic orbitals $\phi_{ui}$, or [MO-LCAO](https://en.wikipedia.org/wiki/Linear_combination_of_atomic_orbitals), see section 2.2.5 of [1]). Atomic orbital is also a wave function which is usually known and have explicit form centered at each atoms. For example, the exact 1s orbital of a hydrogen atom is $\phi(r-R) = (\zeta^3/\pi)^{1/2}e^{-\zeta\|r-R\|}$ where $R$ is the center coodinate of the atom and $\zeta = 1.0$ (Slater orbital). However, for the sake of simpler integral evaluation (积分计算), we usually use Gaussian orbital which has the form $\phi(r-R) = (2\alpha/\pi)^{3/4}e^{-\alpha\|r-R\|^2}$. 

Additionally, when the number of basis functions is $K$, later we will see that we can solve $K$ molecular spatial orbitals $\psi_1, \cdots, \psi_K$ ($2K$ molecular spin orbitals $\chi_1, \cdots, \chi_{2K}$) with orbital energies $\epsilon_1 < \cdots < \epsilon_K$. For the ground state which has the lowest energy, the $N$ electrons will occupy the lowest molecular spin orbitals, and leave the other $2K - N$ orbital unoccupied or virtual.

Now we see how to solve the equation $\eqref{eq2}$ with the expansion $\psi_i(r_i) = \sum_{u=1}^K C_{ui} \phi_u(r_i)$. Substitude the expansion into $\eqref{eq2}$ we have

$$
h(i)\sum_{v=1}^K C_{vi} \phi_v(r_i) = \epsilon_i \sum_{v=1}^K C_{vi} \phi_v(r_i)
$$

Since we have $K$ unknown variables $C_{1i}, \cdots, C_{Ki}$, if we can construct $K$ normal equations then we can solve the $K$ variables out. The important trick here is to multiple $\phi_u(r_i)$ on the left for $u = 1, \cdots, K$ and integrate (see page 29 of [1] for more details). Then we have $K$ equations

$$
\begin{aligned}
\int \sum_{v=1}^K C_{vi} \phi_u(r_i)h(i)\phi_v(r_i)dr_i &= \int \epsilon_i \sum_{v=1}^K C_{vi} \phi_u(r_i)\phi_v(r_i)dr_i, \quad \forall u = 1, \cdots, K \\
\sum_{v=1}^K C_{vi} \underbrace{\int \phi_u(r_i)h(i)\phi_v(r_i)dr_i}_{H_{uv}} &= \epsilon_i \sum_{v=1}^K C_{vi} \underbrace{\int \phi_u(r_i)\phi_v(r_i)dr_i}_{S_{uv}}, \quad \forall u = 1, \cdots, K
\end{aligned}
$$

Since both the basis functions (atomic orbitals) $\phi$ and $h(i)$ are explicitly given, the integral $\int \phi_u(r_i)h(i)\phi_v(r_i)dr_i$ and $\int \phi_u(r_i)\phi_v(r_i)dr_i$ can be calculated as constants (later we will show the explicit expression of these integrals). We denote constant $H_{uv} = \int \phi_u(r_i)h(i)\phi_v(r_i)dr_i$ and $S_{uv} = \int \phi_u(r_i)\phi_v(r_i)dr_i$ (overlap integral), then we have

$$
\sum_{v=1}^K H_{uv} C_{vi} = \epsilon_i \sum_{v=1}^K S_{uv} C_{vi}, \quad \forall u = 1, \cdots, K
$$

We can write the $K$ equations together in matrix form as

$$
\begin{pmatrix}
H_{11} & H_{12} & \cdots & H_{1K} \\
H_{21} & H_{22} & \cdots & H_{2K} \\
\vdots & \vdots & \ddots & \vdots \\
H_{K1} & H_{K2} & \cdots & H_{KK} 
\end{pmatrix}
\begin{pmatrix}
C_{1i} \\ C_{2i} \\ \vdots \\ C_{Ki}
\end{pmatrix}
= \epsilon_i
\begin{pmatrix}
S_{11} & S_{12} & \cdots & S_{1K} \\
S_{21} & S_{22} & \cdots & S_{2K} \\
\vdots & \vdots & \ddots & \vdots \\
S_{K1} & S_{K2} & \cdots & S_{KK} 
\end{pmatrix}
\begin{pmatrix}
C_{1i} \\ C_{2i} \\ \vdots \\ C_{Ki}
\end{pmatrix}
$$

or

$$
\begin{equation}
HC_i = \epsilon_i SC_i \label{eq3}
\end{equation}
$$

where $C_i = (C_{1i}, \cdots, C_{Ki})^T$. If our basis functions are orthonormal (正交, that is, $S_{uv} = \int \phi_u(r)\phi_v(r)dr = \delta_{uv} = \begin{cases}1 & u = v \\\\ 0 & u \neq v\end{cases}$, see 1.2 of [1]), then $S = I$ is an identity matrix. In this case we solve $HC_i = \epsilon_i C_i$, which equals to finding $K$ eigenvector $C_1, \cdots, C_K$ and eigenvalue $\epsilon_1, \cdots, \epsilon_K$ of matrix $H$. Then we get $K$ molecular spatial orbitals $\psi_1(r_1) = \sum_{u=1}^K C_{u1} \phi_u(r_1), \cdots, \psi_K(r_K) = \sum_{u=1}^K C_{uK} \phi_u(r_K)$ in which both coefficients $C$ and basis functions $\phi$ are known.

However, the basis functions are usually not orthonormal ($S \neq I$). In this case we want to find a linear transformation $X$ so that $X^TSX = I$. If we can find such a transformation $X$, let $C_i' = X^{-1}C_i'$ ($C_i = XC_i'$) so we have $HXC_i' = \epsilon_i SXC_i'$. Multiply $X^T$ on the left and we have $\underbrace{X^THX}\_{=H'}C_i' = \epsilon_i \underbrace{X^TSX}_{=I}C_i'$. Let $H' = X^THX$ and we have $H'C_i' = \epsilon_i C_i'$. Then we solve $C_i'$ as usual and do linear transformation $C_i = XC_i'$ to get solution $C_i$ of the original equation $HC_i = \epsilon_i SC_i$.

The following are the steps to find the transformation $X$. We first do matrix diagonalization (矩阵对角化) to $S$, that is, find $K$ eigenvectors $U = [U_1, \cdots, U_K]$ and eigenvalues $s = [s_1, \cdots, s_K]$ so that $SU = U\text{diag}(s) \Rightarrow U^TSU = \text{diag}(s)$ ($U$ is orthonormal and normalized so $U^T = U^{-1}$). Then let $X = U\text{diag}(s^{-1/2})$ in which $s^{-1/2} = [s_1^{-1/2}, \cdots, s_K^{-1/2}]$, we have $X^TSX = \text{diag}(s^{-1/2})U^TSU\text{diag}(s^{-1/2}) = \text{diag}(s^{-1/2})\text{diag}(s)\text{diag}(s^{-1/2}) = I$ (remind that $(AB)^T = B^TA^T$). (see 3.4.5 of [1] for details)

The final step towards actual computation is to calculate $H_{uv} = \int \phi_u(r_i)h(i)\phi_v(r_i)dr_i$ and $S_{uv} = \int \phi_u(r_i)\phi_v(r_i)dr_i$ for Gaussian atomic orbitals $\phi_u(r-R_u) = (2\alpha/\pi)^{3/4}e^{-\alpha\|r-R_u\|^2}$ and $h(i) = -\frac{1}{2}\nabla_i^2 - \sum_{A}\frac{Z_A}{r_{Ai}}$. While the integrals can be calculated analytically, the process is quite complicated. Here we list the results of the integrals directly from appendix A of [1], more details can be found in the [appendix](#integral-evaluation).

$$
\begin{aligned}
S_{uv} &= \int \phi_u(r_i)\phi_v(r_i)dr_i = (\frac{\pi}{p})^{3/2} e^{-\frac{\alpha\beta}{\alpha + \beta}|R_u - R_v|^2} \quad (A.9) \\
H_{uv} &= T_{uv} + \sum_A V_{uv}^A \quad \text{in which}\\
T_{uv} &= \int \phi_u(r_i)-\frac{1}{2}\nabla_i^2\phi_v(r_i)dr_i = \frac{\alpha\beta}{\alpha + \beta}[3 - 2\frac{\alpha\beta}{\alpha + \beta}|R_u - R_v|^2] S_{uv} \quad (A.11) \\
V_{uv}^A &= \int \phi_u(r_i)-\frac{Z_A}{r_{Ai}}\phi_v(r_i)dr_i = -\frac{2\pi}{\alpha + \beta} Z_A e^{-\frac{\alpha\beta}{\alpha + \beta}|R_u - R_v|^2}F_0[(\alpha + \beta)|R_p - R_v|^2] \quad (A.33) \\
F_0(t) &= \frac{1}{2}\sqrt{\frac{\pi}{t}}erf(\sqrt{t})\\
p &= \alpha + \beta\\
R_p &= \frac{\alpha R_{u} + \beta R_{v}}{\alpha + \beta}
\end{aligned}
$$

in which $erf(t)$ is the [Gauss error function](https://en.wikipedia.org/wiki/Error_function).

## Hartree approximation ($V_{ee}$ considered, antisymmetry principle ignored)

Before considering $V_{ee}$, let's derive $HC_i = \epsilon_i SC_i$ $\eqref{eq3}$ directly from the Schrodinger equation $\eqref{eq1}$ via variation principle with apprximated hartree product wave function $\Psi(r_1, \cdots, r_N) = \prod_i \psi_i(r_i)$. 

The variation principle is as follows: for eigenvalue problem $H\Psi = E \Psi$ in which $H = \sum_{i=1}^N h(i)$ is a Hermitian operator and $\Psi$ is a wave function, there exists an infinate set of exact solutions $H\Psi_\alpha = E_\alpha \Psi_\alpha$ where $E_0 \leq E_1 \leq \cdots \leq E_\alpha \leq \cdots$. Then, given an arbitrary normalized wave function $\tilde{\Psi}, \int \|\tilde{\Psi}\|^2 = 1$, the expectation of the Hamiltionian is an upper bound to the exact ground state enengy $E_0$, that is, $\int \tilde{\Psi} H \tilde{\Psi} \geq E_0$. The equality holds only when $\tilde{\Psi} = \Psi_0$. (see section 1.3 of [1] for details)

Therefore, we can transform the solving of $\Psi_0$ into a constrainted optimization problem: $\min \int \Psi H \Psi \text{ s.t. } \int \|\Psi\|^2 = 1$. With $\Psi(r_1, \cdots, r_N) = \prod_i \psi_i(r_i)$, $\int \|\Psi\|^2 = 1$ is transformed to $N$ constraints $\int \|\psi_i\|^2 = 1, i = 1, \cdots, N$. The standard method to solve constrainted optimization problem is to construct a Lagrangian function $\mathcal{L}(\Psi, E) = \int \Psi H \Psi - \sum_i E_i(\int \|\psi_i\|^2 - 1)$ and let $\frac{\partial \mathcal{L}}{\partial \Psi} = 0$.

we first calculate

$$
\begin{aligned}
& \int \Psi(r_1, \cdots, r_N) H \Psi(r_1, \cdots, r_N) dr_1\cdots dr_N \\
=& \sum_{i=1}^N \int_{r_N}\cdots\int_{r_1} \psi(r_1)\cdots\psi(r_N) h(i) \psi(r_1)\cdots\psi(r_N) dr_1\cdots dr_N \\
& \text{Note that $\int \psi_i(r)\psi_i(r) dr = 1$, so} \\
=& \sum_{i=1}^N \int_{r_i} \psi_i(r_i)h(i)\psi_i(r_i) dr_i \\
& \text{with the basis expansion $\psi_i(r_i) = \sum_{u=1}^K C_{ui} \phi_u(r_i)$} \\
=& \sum_{i=1}^N \int \sum_{u, v}C_{ui}C_{vi}\phi_u(r_i)h(i)\phi_v(r_i) dr_i \\
=& \sum_{i=1}^N \sum_{u, v}C_{ui}C_{vi} \underbrace{\int \phi_u(r_i)h(i)\phi_v(r_i) dr_i}_{H_{uv}} = \sum_{i=1}^N \sum_{u, v}C_{ui}C_{vi}H_{uv}\\
\end{aligned}
$$

and 

$$
\begin{aligned}
& \sum_{i=1}^N E_i(\int \psi_i(r_i)\psi_i(r_i) dr_i - 1) \\
=& \sum_{i=1}^N E_i(\sum_{u, v} C_{ui}C_{vi}\underbrace{\int \phi_i(r_i)\phi_i(r_i) dr_i}_{S_{uv}} - 1) \\
=& \sum_{i=1}^N E_i(\sum_{u, v} C_{ui}C_{vi}S_{uv} - 1)
\end{aligned}
$$

Thus

$$
\begin{aligned}
\mathcal{L}(C, E) &= \sum_{i=1}^N [\sum_{u, v}C_{ui}C_{vi}H_{uv} - E_i(\sum_{u, v} C_{ui}C_{vi}S_{uv} - 1)] \\
&= \sum_{i=1}^N [\sum_{u, v}C_{ui}C_{vi}(H_{uv} - E_i S_{uv}) + E_i] \\
\frac{\partial L}{\partial C_{ui}} &= \sum_{v \neq u} 2 C_{ui}(H_{uv} - E_i S_{uv}) + 2 C_{ui}(H_{uv} - E_i S_{uv}) = 0 \\
& \sum_v C_{vi}(H_{uv} - E_i S_{uv}) = 0 \\
& \sum_v H_{uv}C_{vi} = E_i \sum_v S_{uv}C_{vi}, \forall i = 1, \cdots, N, v = 1, \cdots, K
\end{aligned}
$$ 

whose matrix form is identical to $HC_i = \epsilon_i SC_i$ $\eqref{eq3}$. (more details can be found in section 1.3.2 of [1] who assume the basis functions are orthogonal so $S = I$)

Now let's take $V_{ee} = \sum_{i < j}\frac{1}{r_{ij}}$ into account, so we are going to solve

$$
[\sum_i h(i) + \sum_{i < j}\frac{1}{r_{ij}}]\Psi(r_1, \cdots, r_N) = E\Psi(r_1, \cdots, r_N)
$$

in this case $H = \sum_i h(i) + \sum_{i < j}\frac{1}{r_{ij}} = \sum_i h(i) + \frac{1}{2}\sum_{i \neq j}\frac{1}{r_{ij}}$, so we have a new term $\int \Psi (\frac{1}{2}\sum_{i \neq j}\frac{1}{r_{ij}}) \Psi$. That is,

$$
\begin{aligned}
& \int \Psi(r_1, \cdots, r_N) (\frac{1}{2}\sum_{i \neq j} \frac{1}{r_{ij}}) \Psi(r_1, \cdots, r_N) dr_1\cdots dr_N \\
=& \frac{1}{2}\sum_{i \neq j} \int_{r_N}\cdots\int_{r_1} \psi(r_1)\cdots\psi(r_N) \frac{1}{r_{ij}} \psi(r_1)\cdots\psi(r_N) dr_1\cdots dr_N \\
=& \frac{1}{2}\sum_{i \neq j} \int_{r_i, r_j} \psi_i(r_i)\psi_j(r_j)\frac{1}{r_{ij}}\psi_j(r_j)\psi_i(r_i) dr_i dr_j \\
=& \frac{1}{2}\sum_{i \neq j} \sum_{u, v, p, q}C_{ui}C_{pj}C_{qj}C_{vi} \underbrace{\int \phi_u(r_i)\phi_p(r_j)\frac{1}{r_{ij}}\phi_q(r_j)\phi_v(r_i) dr_i dr_j}_{(uv|pq)}\\
\frac{\partial}{\partial C_{ui}} =& 2 \times \frac{1}{2} \sum_{j (\neq i)}\sum_{v, p, q}2 C_{vi}C_{pj}C_{qj}(uv|pq) \\
=& \sum_v C_{vi}[\sum_{j (\neq i)}\sum_{p, q}C_{pj}C_{qj}(uv|pq)]
\end{aligned}
$$

Therefore

$$
\begin{aligned}
& \sum_v C_{vi}(H_{uv} + \sum_{j (\neq i)}\sum_{p, q}C_{pj}C_{qj}(uv|pq) - E_i S_{uv}) = 0 \\
& \sum_v (H_{uv} + \sum_{j (\neq i)}\sum_{p, q}C_{pj}C_{qj}(uv|pq))C_{vi} = E_i \sum_v S_{uv}C_{vi}, \forall i = 1, \cdots, N, v = 1, \cdots, K
\end{aligned}
$$

## Hartree-Fock approximation

## Appendix

### Integral evaluation

More integrals can be found in Appendix A of [1].

For overlap integral $S_{uv} = \int \phi_u(r_i)\phi_v(r_i)dr_i$ with Gaussian atomic orbitals $\phi_u(r-R_u) = (2\alpha/\pi)^{3/4}e^{-\alpha\|r-R_u\|^2}$, we first show that the product of two Gaussians is also a Gaussian. That is

$$
\begin{aligned}
& \phi_u(r-R_u) \phi_v{r - R_v} \\
= & e^{-\alpha|r - R_u|^2} e^{-\beta|r - R_v|^2} \\
= & e^{-(\alpha|r - R_u|^2 + \beta|r - R_v|^2)} \\
= & e^{-[\alpha(r_x - R_{u,x})^2 + \beta(r_x - R_{v,x})^2 + \alpha(r_y - R_{u,y})^2 + \beta(r_y - R_{v,y})^2 + \alpha(r_z - R_{u,z})^2 + \beta(r_z - R_{v,z})^2]} \\
= & e^{-[(\alpha + \beta)r_x^2 - 2(\alpha R_{u,x} + \beta R_{v,x}) + \alpha R_{u,x}^2 + \beta R_{u,y}^2 + \cdots]} \\
= & e^{-[(\alpha + \beta)(r_x - \frac{\alpha R_{u,x} + \beta R_{v,x}}{\alpha + \beta})^2 + \alpha R_{u,x}^2 + \beta R_{u,y}^2 - \frac{(\alpha R_{u,x} + \beta R_{v,x})^2}{\alpha + \beta} + \cdots]} \\
= & e^{-[(\alpha + \beta)(r_x - \frac{\alpha R_{u,x} + \beta R_{v,x}}{\alpha + \beta})^2 + \cdots]} e^{(\alpha + \beta)(\alpha R_{u,x} + \beta R_{v,x}) - (\alpha R_{u,x} + \beta R_{v,x})^2] / (\alpha + \beta) + \cdots} \\
= & e^{-[(\alpha + \beta)(r_x - \frac{\alpha R_{u,x} + \beta R_{v,x}}{\alpha + \beta})^2 + \cdots]} e^{-\frac{\alpha\beta}{\alpha + \beta}(R_{u,x}-R_{v,x})^2 + \cdots} \\
= & e^{-p|r - R_p|^2} \tilde{K} \\
= & \tilde{K} \phi_p(r - R_p)
\end{aligned}
$$

in which $p = \alpha + \beta$, $R_p = \frac{\alpha R_{u} + \beta R_{v}}{\alpha + \beta}$ is the center of the new Gaussian, and $\tilde{K} = e^{-\frac{\alpha\beta}{\alpha + \beta}\|R_u - R_v\|^2}$.

Then

$$
S_{uv} = \int \phi_u(r_i)\phi_v(r_i)dr_i = \tilde{K} \int \phi_p(\textbf{r}_i - \textbf{R}_p) d\textbf{r}_i = \tilde{K} \int e^{-p|\textbf{r}_i - \textbf{R}_p|^2} d\textbf{r}_i
$$

Let $\textbf{r} = \textbf{r}_i - \textbf{R}_p$, reminds that [triple integrals in spherical coordinates](https://sxyd.sdut.edu.cn/_upload/tpl/02/32/562/template562/onlineLearning/gaodengshuxuexia/lesson/9.5jisuansanchongjifen.htm) $d\textbf{r} = (r\sin\varphi d\theta)(r d\varphi)dr = r^2\sin\varphi dr d\varphi d\theta$, then

$$
\begin{aligned}
\int e^{-p|\textbf{r}_i - \textbf{R}_p|^2} d\textbf{r}_i &= \int e^{-p|\textbf{r}|^2} d\textbf{r} \\
&= \int_{r=0}^{+\infty} \int_{\varphi = 0}^\pi \int_{\theta = 0}^{2\pi} e^{-pr^2} r^2\sin\varphi dr d\varphi d\theta \\
&= 4\pi \int_{r=0}^{+\infty} e^{-pr^2} r^2 dr
\end{aligned}
$$

To calculate $\int_{r=0}^{+\infty} e^{-pr^2} r^2 dr$, reminds that the Gaussian distribution $\int_{x=-\infty}^{\infty} \frac{1}{\sigma \sqrt{2\pi}} e^{-\frac{x^2}{2\sigma^2}} = 1$. Let $\sigma = \sqrt{\frac{1}{2\alpha}}$ then we have a more general form $\int_{x=-\infty}^{\infty} e^{-\alpha x^2} = \sqrt{\frac{\pi}{\alpha}}$. [Different each side by parameter](http://www.umich.edu/~chem461/Gaussian%20Integrals.pdf) $\alpha$ we have $\int_{x=-\infty}^{\infty} -x^2 e^{-\alpha x^2} = \frac{\sqrt{\pi}}{-2\alpha^{3/2}}$, therefore $\int_{x=-\infty}^{\infty} x^2 e^{-\alpha x^2} = \frac{\sqrt{\pi}}{2\alpha^{3/2}}$ and $\int_{x=0}^{\infty} -x^2 e^{-\alpha x^2} = \frac{\sqrt{\pi}}{4\alpha^{3/2}}$ by symmetry. The final result of $S_{uv}$ is

$$
S_{uv} = 4\pi \tilde{K} \frac{\sqrt{\pi}}{4p^{3/2}} = (\frac{\pi}{p})^{3/2} e^{-\frac{\alpha\beta}{\alpha + \beta}|R_u - R_v|^2}
$$

## References

1. Szabo, A., & Ostlund, N. S. (1982). Modern quantum chemistry: introduction to advanced electronic structure theory. Dover Publication.
2. Sherrill, C. D. (2000). [An introduction to Hartree-Fock molecular orbital theory](http://vergil.chemistry.gatech.edu/courses/chem6485/pdf/hf-intro.pdf). School of Chemistry and Biochemistry, Georgia Institute of Technology.
3. https://gitlab.com/LLindenbauer/Hartree-Fock-Tutorial
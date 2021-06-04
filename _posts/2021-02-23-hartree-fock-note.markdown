---
layout: post
title:  "Hartree-Fock Theory Note"
author: Snowkylin
categories: physics quantum ab-initio
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

in which $\nabla_t^2 = \frac{\partial^2}{\partial r_{i, x}^2} + \frac{\partial^2}{\partial r_{i, y}^2} + \frac{\partial^2}{\partial r_{i, z}^2}$ is the laplace operator for $r_i = (r_{i, x}, r_{i, y}, r_{i, z})$, $N$ is the number of electrons. $r_i, r_j$ and $R_A, R_B$ are the 3d coodinate of electrons $\_i, \_j$ (电子) and nuciels $\_A, \_B$ (原子核) respectively. $r_{iA} = \|r_i - R_A\|$ is the distance between electron $i$ and nuciel $A$, $r_{ij} = \|r_i - r_j\|$ is the distance between electron $i$ and $j$. $Z_A$ is the atomic number (原子序数) of nucleus $A$. 

- $T_e(r)$: kinetic energy of electrons
- $V_{eN}(r, R)$: electrostatic potential (静电势) between electrons ($_e$) and nuciels ($_N$) (or "external potentials")
- $V_{ee}(r)$: electrostatic repulsion (静电排斥) between electrons

The solution of the equation is a "wave function" $\Psi(r_1, \cdots, r_N)$ so that the equation always holds for any $r_1, \cdots, r_N$. Additionally, $\|\Psi(r_1, \cdots, r_N)\|^2$ is the probability that electron 1 appear at coodinate $r_1$ and electron 2 appears at coodinate $r_2$ and so on.

Here we assume that the nuciels are fixed so all $R$ are constants (the Born-Oppenheimer approximation). Therefore we do not consider $T_N(R) = \sum_A \frac{1}{2M_A}\nabla_A^2$ (kinetic energy of nucleis) and $V_{NN}(R) = \sum_{A < B}\frac{Z_A Z_B}{R_{AB}}$ (Coulombic repulsion between nucleis) which are also constants.

Let $h(i) = -\frac{1}{2}\nabla_i^2 - \sum_{A}\frac{Z_A}{r_{Ai}}$, then the equation can be also written as

$$
[\sum_i h(i) + \sum_{i < j}\frac{1}{r_{ij}}]\Psi(r_1, \cdots, r_N) = E\Psi(r_1, \cdots, r_N)
$$

There are two challenges to solve this equation:

1. $V_{ee}(r) = \sum_{i < j}\frac{1}{r_{ij}}$ is hard to tackle, since it makes all electrons correlated. Without it, we can simply write the equation as $\sum_i h(i) \Psi(r_1, \cdots, r_N) = E\Psi(r_1, \cdots, r_N)$. In this case, we only need to solve $N$ single-electron equations $h(i)\psi_i(r_i) = \epsilon_i\psi_i(r_i)$ for $i = 1, \cdots, N$, get $N$ wave functions $\psi_1(r_1), \cdots, \psi_N(r_N)$ (molecular spatial orbitals (空间轨道). *Orbital* is a wave function for a single electron, see section 2.2 of [1]), then let $\Psi(r_1, \cdots, r_N) = \prod_i \psi_i(r_i)$ (hartree product), $E = \sum_i \epsilon_i$, and the equation is solved.
2. The wave function $\Psi(r_1, \cdots, r_N)$ needs to satisfy a specific physical property named *antisymmetry principle*, whose famous representation is *Pauli exclusion principle* (泡利不相容原理) that tell us two electrons in the same spatial orbital must have opposite spins. To illustrate this, we need to take spin $\omega = \\\{\alpha, \beta\\\}$ (自旋) into account. Let space-spin coordinates $x = (r, \omega)$ and change the molecular spatial orbital $\psi(r)$ to molecular spin orbital (自旋轨道) $\chi(x) = \psi(r)\omega$. Then the antisymmetry principle requires that $\Psi(\cdots, x_i, \cdots, x_j, \cdots) = -\Psi(\cdots, x_j, \cdots, x_i, \cdots)$. e.g., for a two-electron system, $\Psi(x_1, x_2) = -\Psi(x_2, x_1)$, and when $x_1 = x_2$ we have $\Psi(x_1, x_2) = -\Psi(x_1, x_2) \Rightarrow \Psi(x_1, x_2)  = 0 \Rightarrow \|\Psi(x_1, x_2)\|^2 = 0$, which means the probability of two electrons in the same space with parallel spin is zero.

The Hartree-Fock theory tackles these challenges by the following ways:

1. (Hartree Approximation) For the first challenge, assume $\Psi(r_1, \cdots, r_N) = \prod_{i=1}^N \psi_i(r_i)$ (hartree product, note that we may have $K > N$ molecular spatial orbitals $\psi_1, \cdots, \psi_K$ but we select the first $N$ orbitals with the lowest energy. It is valid since $\int \psi_i(r_i) dr_i = 1$ and $\int \prod_{i=1}^N \psi_i(r_i) dr_1\cdots dr_N = 1$. This is an approximated wave function), then we minimize the total energy of $\eqref{eq1}$ using variational method, and find that: with the assumed wave function, for electron $i$, we can approximate its interaction with other electrons by the average field of other electrons, and we add all the average field to approximate $V_{ee}$. i.e., let $V_{ee} = \sum_i g(i)$. In this case we can let $h'(i) = h(i) + g(i)$ so we can write the equation as $\sum_i h'(i) \Psi(r_1, \cdots, r_N) = E\Psi(r_1, \cdots, r_N)$ and solve it easily as single-electron equations $h'(i)\psi_i(r_i) = \epsilon_i\psi_i(r_i)$ (not so easy though, since $g(i)$ includes the wave function of other electrons, and a self-consistent iteration is needed).
2. (Hartree-Fock Approximation) For the second challenge, assume $\Psi(x_1, \cdots, x_N) = \frac{1}{\sqrt{N!}}\begin{vmatrix}\chi_1(x_1) & \chi_2(x_1) & \cdots & \chi_N(x_1)\\\\ \chi_1(x_2) & \chi_2(x_2) & \cdots & \chi_N(x_2)\\\\ \vdots & \vdots & \ddots & \vdots\\\\ \chi_1(x_N) & \chi_2(x_N) & \cdots & \chi_N(x_N)\end{vmatrix}$ (slater determinant), which is a better approximation of the wave function that satisfy antisymmetry principle. E.g., for the two-electron case, $\Psi(x_1, x_2) = \frac{1}{\sqrt{2}}\begin{vmatrix}\chi_1(x_1) & \chi_2(x_1) \\\\ \chi_1(x_2) & \chi_2(x_2)\end{vmatrix} = \frac{1}{\sqrt{2}}[\chi_1(x_1)\chi_2(x_2) - \chi_2(x_1)\chi_1(x_2)]$. It is easy to check that $\Psi(x_1, x_2) = -\Psi(x_2, x_1)$, and if the two electrons are described by the same spin orbital $\chi = \chi_1 = \chi_2$, then $\Psi(x_1, x_2) = 0$.

## The simplest case (both $V_{ee}$ and antisymmetry principle ignored)

As mentioned before, if both $V_{ee}$ and antisymmetry principle are ignored, then we can just solve the following one-electron differential equation

$$
\begin{equation}
h(i)\psi_i(r_i) = \epsilon_i\psi_i(r_i) \label{eq2}
\end{equation}
$$

The solution of a differential equation is a function. That is, we want to find some functions $\psi_i(r_i)$ (molecular orbicals) so that the above equation holds for all $r_1 \in \mathbb{R}^3$.

However, such equation can be hard to solve in function space. Therefore we want to expand the wave function (molecular orbical) as the linear conbination of some (let's say, $K$) basis functions, 

$$
\psi_i(r_i) = \sum_{u=1}^K C_{ui} \phi_u(r_i)
$$

Then the problem of solving $\psi$ (in function space) transforms to solving $K$ coefficients $C_{1i}, \cdots, C_{Ki}$ (in vector space $\mathbb{R}^K$), which can be easier to solve with linear algebra.

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
\begin{equation}
\sum_{v=1}^K H_{uv} C_{vi} = \epsilon_i \sum_{v=1}^K S_{uv} C_{vi}, \quad \forall u = 1, \cdots, K \label{simplest_raw} 
\end{equation}
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

### Preliminaries: variation principle

Before considering $V_{ee}$, let's derive $HC_i = \epsilon_i SC_i$ $\eqref{eq3}$ directly from the Schrodinger equation $\eqref{eq1}$ via variation principle with apprximated hartree product wave function $\Psi(r_1, \cdots, r_N) = \prod_i \psi_i(r_i)$. 

The variation principle is as follows: for eigenvalue problem $H\Psi = E \Psi$ in which $H = \sum_{i=1}^N h(i)$ is a Hermitian operator and $\Psi$ is a wave function, there exists an infinate set of exact solutions $H\Psi_\alpha = E_\alpha \Psi_\alpha$ where $E_0 \leq E_1 \leq \cdots \leq E_\alpha \leq \cdots$. Then, given an arbitrary normalized wave function $\tilde{\Psi}, \int \|\tilde{\Psi}\|^2 = 1$, the expectation of the Hamiltionian is an upper bound to the exact ground state enengy $E_0$, that is, $\int \tilde{\Psi} H \tilde{\Psi} \geq E_0$. The equality holds only when $\tilde{\Psi} = \Psi_0$. (see section 1.3 of [1] for details)

Therefore, we can transform the solving of $\Psi_0$ into a constrainted optimization problem: $\min \int \Psi H \Psi \text{ s.t. } \int \|\Psi\|^2 = 1$. With $\Psi(r_1, \cdots, r_N) = \prod_i \psi_i(r_i)$, $\int \|\Psi\|^2 = 1$ is transformed to $N$ constraints $\int \|\psi_i\|^2 = 1, i = 1, \cdots, N$. The standard method to solve constrainted optimization problem is to construct a Lagrangian function $\mathcal{L}(\Psi, E) = \int \Psi H \Psi - \sum_i E_i(\int \|\psi_i\|^2 - 1)$ and let $\frac{\partial \mathcal{L}}{\partial \Psi} = 0$.

we first calculate

$$
\begin{align}
& \int \Psi(r_1, \cdots, r_N) H \Psi(r_1, \cdots, r_N) dr_1\cdots dr_N \nonumber \\
=& \sum_{i=1}^N \int_{r_N}\cdots\int_{r_1} \psi(r_1)\cdots\psi(r_N) h(i) \psi(r_1)\cdots\psi(r_N) dr_1\cdots dr_N \nonumber \\
& \text{Note that $\int \psi_i(r)\psi_i(r) dr = 1$ and $h(i)$ is only related to $r_i$, so} \nonumber \\
=& \sum_{i=1}^N \int_{r_i} \psi_i(r_i)h(i)\psi_i(r_i) dr_i \label{E_h} \\
& \text{with the basis expansion $\psi_i(r_i) = \sum_{u=1}^K C_{ui} \phi_u(r_i)$} \nonumber \\
=& \sum_{i=1}^N \int \sum_{u, v}C_{ui}C_{vi}\phi_u(r_i)h(i)\phi_v(r_i) dr_i \nonumber \\
=& \sum_{i=1}^N \sum_{u, v}C_{ui}C_{vi} \underbrace{\int \phi_u(r_i)h(i)\phi_v(r_i) dr_i}_{H_{uv}} = \sum_{i=1}^N \sum_{u, v}C_{ui}C_{vi}H_{uv} \nonumber \\
\end{align}
$$

and 

$$
\begin{aligned}
& \sum_{i=1}^N E_i(\int \psi_i(r_i)\psi_i(r_i) dr_i - 1) \\
=& \sum_{i=1}^N E_i(\sum_{u, v} C_{ui}C_{vi}\underbrace{\int \phi_u(r_i)\phi_v(r_i) dr_i}_{S_{uv}} - 1) \\
=& \sum_{i=1}^N E_i(\sum_{u, v} C_{ui}C_{vi}S_{uv} - 1)
\end{aligned}
$$

Thus

$$
\begin{align*}
\mathcal{L}(C, E) &= \sum_{i=1}^N [\sum_{u, v}C_{ui}C_{vi}H_{uv} - E_i(\sum_{u, v} C_{ui}C_{vi}S_{uv} - 1)] \\
&= \sum_{i=1}^N [\sum_{u, v}C_{ui}C_{vi}(H_{uv} - E_i S_{uv}) + E_i]\\
\frac{\partial L}{\partial C_{ui}} &= \sum_{v \neq u} 2 C_{ui}(H_{uv} - E_i S_{uv}) + 2 C_{ui}(H_{uv} - E_i S_{uv}) = 0 \\
& 2\sum_v C_{vi}(H_{uv} - E_i S_{uv}) = 0 \\
& \sum_v H_{uv}C_{vi} = E_i \sum_v S_{uv}C_{vi}, \forall i = 1, \cdots, N, u = 1, \cdots, K
\end{align*}
$$ 

which is identical to $\eqref{simplest_raw}$ whose matrix form is $HC_i = \epsilon_i SC_i$ $\eqref{eq3}$. (more details can be found in section 1.3.2 of [1] who assume the basis functions are orthogonal so $S = I$)

### Hartree approximation with variation principle

Now let's take $V_{ee} = \sum_{i < j}\frac{1}{r_{ij}}$ into account, so we are going to solve

$$
[\sum_i h(i) + \sum_{i < j}\frac{1}{r_{ij}}]\Psi(r_1, \cdots, r_N) = E\Psi(r_1, \cdots, r_N)
$$

in this case $H = \sum_i h(i) + \sum_{i < j}\frac{1}{r_{ij}} = \sum_i h(i) + \frac{1}{2}\sum_{i \neq j}\frac{1}{r_{ij}}$, so we have a new term $\int \Psi (\frac{1}{2}\sum_{i \neq j}\frac{1}{r_{ij}}) \Psi$. That is,

$$
\begin{align}
& \int \Psi(r_1, \cdots, r_N) (\frac{1}{2}\sum_{i \neq j} \frac{1}{r_{ij}}) \Psi(r_1, \cdots, r_N) dr_1\cdots dr_N \nonumber \\
=& \frac{1}{2}\sum_{i \neq j} \int_{r_N}\cdots\int_{r_1} \psi(r_1)\cdots\psi(r_N) \frac{1}{r_{ij}} \psi(r_1)\cdots\psi(r_N) dr_1\cdots dr_N \nonumber \\
=& \frac{1}{2}\sum_{i \neq j} \int_{r_i, r_j} \psi_i(r_i)\psi_j(r_j)\frac{1}{r_{ij}}\psi_j(r_j)\psi_i(r_i) dr_i dr_j \label{E_coulomb} \\
=& \frac{1}{2}\sum_{i \neq j} \sum_{u, v, \lambda, \sigma}C_{ui}C_{\lambda j}C_{\sigma j}C_{vi} \underbrace{\int \phi_u(r_i)\phi_\lambda(r_j)\frac{1}{r_{ij}}\phi_\sigma(r_j)\phi_v(r_i) dr_i dr_j}_{(uv|\lambda\sigma), \text{see (3.156) of [1]}}  \label{E_coulomb_C} \\
\frac{\partial}{\partial C_{ui}} =& 2 \times \frac{1}{2} \sum_{j (\neq i)}\sum_{v, \lambda, \sigma}2 C_{vi}C_{\lambda j}C_{\sigma j}(uv|\lambda\sigma) \nonumber \\
=& \sum_v 2C_{vi}[\sum_{j (\neq i)}\sum_{\lambda, \sigma}C_{\lambda j}C_{\sigma j}(uv|\lambda\sigma)] \nonumber 
\end{align}
$$

(Here basis index $u, v$ correspond to electron $i$, $\lambda, \sigma$ correspond to electron $j$. Please note that we have a factor of 2 before the derivative, since $\sum_{i, j}(i \neq j)$ has two indices which are both possible to equal to $i$. Imaging a square matrix with $i$th row and $i$th column)

Therefore

$$
\begin{align}
& \frac{\partial L}{\partial C_{ui}} = 2\sum_v C_{vi}(H_{uv} + \sum_{j (\neq i)}\sum_{\lambda, \sigma}C_{\lambda j}C_{\sigma j}(uv|\lambda\sigma) - E_i S_{uv}) = 0 \nonumber\\
& \sum_v (H_{uv} + \sum_{j (\neq i)}\sum_{\lambda, \sigma}C_{\lambda j}C_{\sigma j}(uv|\lambda\sigma))C_{vi} = E_i \sum_v S_{uv}C_{vi}, \forall i = 1, \cdots, N, u = 1, \cdots, K \label{hartree_raw}
\end{align}
$$

$\eqref{hartree_raw}$ is the raw form of Hartree approximation. We will discuss how to transform it to matrix form [in later section](#matrix-form-and-charge-density-matrix).

Now let's see if we replace $h(i)$ with $h(i) + g(i)$ in which 

$$g(i) = \sum_{j (\neq i)}\int \frac{|\psi_j(r_j)|^2}{r_{ij}} dr_j$$

(notice that $\|\psi_j(r_j)\|^2$ is the possibility density of electron $j$ that appears at coordinate $r_j$, $\int \frac{\|\psi_j(r_j)\|^2}{r_{ij}} dr_j$ is the repulsion of electron $j$ towards electron $i$, so $g(i)$ is the average field of all other electrons), then the matrix form of $[h(i) + g(i)]\psi_i(r_i) = \epsilon_i\psi_i(r_i)$ with basis expansion $\psi_i(r_i) = \sum_{u=1}^K C_{ui} \phi_u(r_i)$ will result in the same result above. Similar to the simplest case discussed before, we write $g(i)\psi_i(r_i)$ as $g(i)\sum_v C_{vi} \phi_v(r_i) = \sum_v C_{vi} g(i)\phi_v(r_i)$, left multiplied by $\phi_u(r_i)$ and integrate. We have

$$
\begin{align*}
&\sum_v C_{vi}\int \phi_u(r_i)g(i)\phi_v(r_i) dr_i \\
=& \sum_v C_{vi}\int \phi_u(r_i)[\sum_{j (\neq i)}\int \psi_j(r_j)\frac{1}{r_{ij}}\psi_j(r_j) dr_j]\phi_v(r_i) dr_i \\
=& \sum_v C_{vi}\int \phi_u(r_i)[\sum_{j (\neq i)}\sum_{\lambda, \sigma}C_{\lambda j}C_{\sigma j}\int \phi_\lambda(r_j)\frac{1}{r_{ij}}\phi_\sigma(r_j) dr_j]\phi_v(r_i) dr_i \\
=& \sum_v C_{vi}\sum_{j (\neq i)}\sum_{\lambda, \sigma}C_{\lambda j}C_{\sigma j} \int \phi_u(r_i) \phi_\lambda(r_j)\frac{1}{r_{ij}}\phi_\sigma(r_j) \phi_v(r_i) dr_i dr_j \\
=& \sum_v C_{vi}\sum_{j (\neq i)}\sum_{\lambda, \sigma}C_{\lambda j}C_{\sigma j} (uv|\lambda\sigma)
\end{align*}
$$

## Hartree-Fock approximation

Now let's replace the simple hartree product wave function $\Psi(r_1, \cdots, r_N) = \prod_i \psi_i(r_i)$ with the better (single) slater determinant $\Psi(x_1, \cdots, x_N) = \frac{1}{\sqrt{N!}}\begin{vmatrix}\chi_1(x_1) & \chi_2(x_1) & \cdots & \chi_N(x_1)\\\\ \chi_1(x_2) & \chi_2(x_2) & \cdots & \chi_N(x_2)\\\\ \vdots & \vdots & \ddots & \vdots\\\\ \chi_1(x_N) & \chi_2(x_N) & \cdots & \chi_N(x_N)\end{vmatrix}$ ($x_i = (\omega_i, r_i)$ where $\omega_i$ is the spin direction). For example, when $N = 2$, the hartree product wave function is $\Psi(r_1, r_2) = \psi_1(r_1)\psi_2(r_2)$ while the slater determinant wave function will have the form of $\Psi(x_1, x_2) = \frac{1}{2}[\chi_1(x_1)\chi_2(x_2) - \chi_1(x_2)\chi_2(x_1)]$.

Again, we consider the constrainted optimization problem: $\min \int \Psi (\sum_i h(i) + \frac{1}{2}\sum_{i \neq j}\frac{1}{r_{ij}}) \Psi \text{ s.t. } \int \|\Psi\|^2 = 1$. The only difference is that the wave function is replaced with a (single) slater determinant. Assuming that we already calculated the objective (expectation value of energy) 

$$
\begin{align*}
E_0 &= \int \Psi (\sum_i h(i) + \frac{1}{2}\sum_{i \neq j}\frac{1}{x_{ij}}) \Psi \\
&= \underbrace{\sum_{i=1}^N \int \chi_i(x) h(i) \chi_i(x) dx}_{\eqref{E_h}\text{, the simplest case, or $\sum_i[i|h|i]$}} + \underbrace{\frac{1}{2}\sum_{i, j} \int \chi_i(x_1)\chi_i(x_1)\frac{1}{r_{12}}\chi_j(x_2)\chi_j(x_2) dx_1 dx_2}_{\eqref{E_coulomb}\text{, added in Hartree approx., or $\frac{1}{2}\sum_{i, j} [ii|jj]$}} \\
&- \underbrace{\frac{1}{2}\sum_{i, j} \int \chi_i(x_1)\chi_j(x_1)\frac{1}{r_{12}}\chi_j(x_2)\chi_i(x_2) dx_1 dx_2}_{\text{New "exchange" term, or $\frac{1}{2}\sum_{i, j} [ij|ji]$}}
\end{align*}
$$

This result can be found in (2.111) and (3.39) of [1]. Note that we replace spatial orbitals $\psi_i(r)$ with spin orbitals $\chi_i(x)$, and have several minor changes of notation for simplicity:

- $r_i, r_j$ is replaced by $r_1, r_2$
- We write spin orbitals with $r_1$ before $r_{12}^{-1}$, and spin orbitals with $r_2$ after $r_{12}^{-1}$.
- $\sum_{i \neq j}$ is replaced by $\sum_{i, j}$, since the $i = j$ terms in the second term (coulomb) and third term (exchange) cancel each other out.

Now we are going to transit the spin orbitals to spatial orbitals. Note that $\chi_i(x) = \alpha(\omega)\psi_i(r) \text{ or } \beta(\omega)\psi_i(r), x = (\omega, r)$ and the two spin wave functions $\alpha(\omega)$ and $\beta(\omega)$ are orthogonal ($\int \alpha(\omega)\alpha(\omega)d\omega = \int \beta(\omega)\beta(\omega)d\omega = 1$, $\int \alpha(\omega)\beta(\omega)d\omega = 0$)

By spliting $x$ to $\omega$ and $r$, since $h(i)$ is only related with $r$ and has no relationship with $\omega$, we can check that

$$
\begin{align*}
\int \chi_i(x) h(i) \chi_i(x) dx &= \int_r \int_\omega \alpha(\omega)\psi_i(r)h(i)\alpha(\omega)\psi_i(r) d\omega dr \\
&= \int_r [\underbrace{\int_\omega \alpha(\omega)\alpha(\omega)d\omega}_{=1}]\psi_i(r)h(i)\psi_i(r) dr \\
&= \int \psi_i(r)h(i)\psi_i(r) dr
\end{align*}
$$

Replacing $\alpha(\omega)$ with $\beta(\omega)$ will not change the result. We can also use a simpler notation $[i\|h\|i] = (i\|h\|i)$ ($[]$ is for spin orbitals and $()$ is for spatial orbitals)

In a similar way, for the second term (coulomb) we have $\int \chi_i(x_1)\chi_i(x_1)\frac{1}{r_{12}}\chi_j(x_2)\chi_j(x_2) dx_1 dx_2 = \int \psi_i(r_1)\psi_i(r_1)\frac{1}{r_{12}}\psi_j(r_2)\psi_j(r_2) dr_1 dr_2$ (or $[ii\|jj] = (ii\|jj)$). However, for the third term (exchange), things become complicated. We cannot compute it unless we know the exact spin direction ($\alpha(\omega)$ or $\beta(\omega)$) for each spin orbital $\chi_i$. 

Assuming we are in a *closed-shell restricted* scenario. That is, we have an even number of electrons, filling the spatial orbitals from bottom to up, each spatial orbitals contain two electrons with opposite spin, such as $\chi_1(x_1) = \alpha(\omega_1)\psi_1(r_1)$, $\chi_2(x_2) = \beta(\omega_1)\psi_1(r_1)$, $\chi_3(x_3) = \alpha(\omega_3)\psi_2(r_3)$, $\chi_4(x_4) = \beta(\omega_4)\psi_2(r_4)$, etc. In this case the first and second term become $2 \sum_{i=1}^{N/2} \int \psi_i(r)h(i)\psi_i(r) dr$ and $2 \sum_{i=1}^{N/2} \sum_{j=1}^{N/2} \int \psi_i(r_1)\psi_i(r_1)\frac{1}{r_{12}}\psi_j(r_2)\psi_j(r_2) dr_1 dr_2$ ($2 = 2 \times 2 \times \frac{1}{2}$) respectively. For the third term (exchange), both electrons $i$ and $j$ can be in spin $\alpha$ or $\beta$, then we discuss it in four cases ($ij\rightarrow\alpha\alpha, ij\rightarrow\beta\alpha, ij\rightarrow\alpha\beta, ij\rightarrow\beta\beta$):

$$
\begin{align*}
& \frac{1}{2}\sum_{i=1}^N \sum_{j=1}^N \int \chi_i(x_1)\chi_j(x_1)\frac{1}{r_{12}}\chi_j(x_2)\chi_i(x_2) dx_1 dx_2 \\
=& \frac{1}{2}\sum_{i=1}^{N/2} \sum_{j=1}^{N/2} \int {\color{red}{\alpha(\omega_1)}}\psi_i(r_1){\color{red}{\alpha(\omega_1)}}\psi_j(r_1)\frac{1}{r_{12}}{\color{red}{\alpha(\omega_2)}}\psi_j(r_2){\color{red}{\alpha(\omega_2)}}\psi_i(r_2) d\omega_1 dr_1 d\omega_2 dr_2 \\
=& \frac{1}{2}\sum_{i=1}^{N/2} \sum_{j=1}^{N/2} \int {\color{blue}{\beta(\omega_1)}}\psi_i(r_1){\color{red}{\alpha(\omega_1)}}\psi_j(r_1)\frac{1}{r_{12}}{\color{red}{\alpha(\omega_2)}}\psi_j(r_2){\color{blue}{\beta(\omega_2)}}\psi_i(r_2) d\omega_1 dr_1 d\omega_2 dr_2 \\
=& \frac{1}{2}\sum_{i=1}^{N/2} \sum_{j=1}^{N/2} \int {\color{red}{\alpha(\omega_1)}}\psi_i(r_1){\color{blue}{\beta(\omega_1)}}\psi_j(r_1)\frac{1}{r_{12}}{\color{blue}{\beta(\omega_2)}}\psi_j(r_2){\color{red}{\alpha(\omega_2)}}\psi_i(r_2) d\omega_1 dr_1 d\omega_2 dr_2 \\
=& \frac{1}{2}\sum_{i=1}^{N/2} \sum_{j=1}^{N/2} \int {\color{blue}{\beta(\omega_1)}}\psi_i(r_1){\color{blue}{\beta(\omega_1)}}\psi_j(r_1)\frac{1}{r_{12}}{\color{blue}{\beta(\omega_2)}}\psi_j(r_2){\color{blue}{\beta(\omega_2)}}\psi_i(r_2) d\omega_1 dr_1 d\omega_2 dr_2
\end{align*}
$$

The second and third term will vanish since $\int {\color{red}{\alpha(\omega)}}{\color{blue}{\beta(\omega)}}d\omega = 0$. For the other terms, the spin wave function vanished, and the final result is $\sum_{i=1}^{N/2} \sum_{j=1}^{N/2} \int \psi_i(r_1)\psi_j(r_1)\frac{1}{r_{12}}\psi_j(r_2)\psi_i(r_2) dr_1 dr_2$. This reflect the fact that there is only an exchange interaction between electrons of parallel spin. See details in Chapter 2.3.5 (page 81) and 3.4.1 (page 133) of [1].

Now we expand the exchange term with basis functions and take derivative

$$
\begin{align*}
&\sum_{i=1}^{N/2} \sum_{j=1}^{N/2} \int \psi_i(r_1)\psi_j(r_1)\frac{1}{r_{12}}\psi_j(r_2)\psi_i(r_2) dr_1 dr_2 \\
=& \sum_{i=1}^{N/2} \sum_{j=1}^{N/2} \sum_{u, v, \lambda, \sigma}C_{ui}C_{vi}C_{\lambda j}C_{\sigma j}\underbrace{\int \phi_u(r_1)\phi_\lambda(r_1)\frac{1}{r_{12}}\phi_\sigma(r_2)\phi_v(r_2) dr_1 dr_2}_{(u\lambda|\sigma v)} \\
\frac{\partial}{\partial C_{ui}} =& 4\sum_{j=1}^{N/2} \sum_{v, \lambda, \sigma} C_{vi}C_{\lambda j}C_{\sigma j} (u\lambda|\sigma v)
\end{align*}
$$

Therefore

$$
\begin{align}
& \frac{\partial L}{\partial C_{ui}} = \sum_v C_{vi}(4 H_{uv} + 8 \sum_{j=1}^{N/2}\sum_{\lambda, \sigma}C_{\lambda j}C_{\sigma j}(uv|\lambda\sigma) - 4 \sum_{j=1}^{N/2} \sum_{\lambda, \sigma} C_{\lambda j}C_{\sigma j} (u\lambda|\sigma v) - E_i S_{uv}) = 0 \nonumber\\
& \sum_v [H_{uv} + \sum_{j=1}^{N/2} (2 \sum_{\lambda, \sigma}C_{\lambda j}C_{\sigma j}(uv|\lambda\sigma) - \sum_{\lambda, \sigma} C_{\lambda j}C_{\sigma j}(u\lambda|\sigma v))]C_{vi} = E_i \sum_v S_{uv}C_{vi} \label{hf_raw}\\
& \forall i = 1, \cdots, N, u = 1, \cdots, K \nonumber
\end{align}
$$

$\eqref{hf_raw}$ is the raw form of Hartree-Fock approximation. We will discuss how to transform it to matrix form [in later section](#matrix-form-and-charge-density-matrix).

### Exchange term as an operator

Now let's see if it is possible to use an operator $k(i)$ (similar to $g(i)$ in Hartree approximation) to represent the additional exchange term in the above equation, so that the matrix form of $[h(i) + g(i) + k(i)]\psi_i(r_i) = \epsilon_i\psi_i(r_i)$  with basis expansion $\psi_i(r_i) = \sum_{u=1}^K C_{ui} \phi_u(r_i)$ will result in the same result above. Actually it is possible to define $k(i)$ in the following way

$$
k(i){\color{red}\psi_{\color{red}i}}(r_i) = \sum_{j=1}^{N/2}[\int {\color{blue}\psi_{\color{blue}j}}(r_j) r_{ij}^{-1} {\color{red}\psi_{\color{red}i}}(r_j)] {\color{blue}\psi_{\color{blue}j}}(r_i)
$$

compared with former $g(i)$

$$
g(i){\color{red}\psi_{\color{red}i}}(r_i) = 2\sum_{j=1}^{N/2}[\int {\color{blue}\psi_{\color{blue}j}}(r_j) r_{ij}^{-1} {\color{blue}\psi_{\color{blue}j}}(r_j)] {\color{red}\psi_{\color{red}i}}(r_i)
$$

Similarly, we write $k(i)\psi_i(r_i)$ as $k(i)\sum_v C_{vi} \phi_v(r_i) = \sum_v C_{vi} k(i)\phi_v(r_i)$, left multiplied by $\phi_u(r_i)$ and integrate. We have

$$
\begin{align*}
&\sum_v C_{vi}\int \phi_u(r_i)k(i)\phi_v(r_i) dr_i \\
=& \sum_v C_{vi}\int \phi_u(r_i)[\sum_{j=1}^{N/2}\int \psi_j(r_j)\frac{1}{r_{ij}}\phi_v(r_j) dr_j]\psi_j(r_i) dr_i \\
=& \sum_v C_{vi}\sum_{\lambda}C_{\lambda j}\int \phi_u(r_i)[\sum_{j=1}^{N/2}\sum_{\sigma}C_{\sigma j}\int \phi_\sigma(r_j)\frac{1}{r_{ij}}\phi_v(r_j) dr_j]\phi_\lambda(r_i) dr_i \\
=& \sum_v C_{vi}\sum_{j=1}^{N/2}\sum_{\lambda, \sigma}C_{\lambda j}C_{\sigma j}\int \phi_u(r_i)\phi_\lambda(r_i)\frac{1}{r_{ij}} \phi_\sigma(r_j) \phi_v(r_j) dr_i dr_j \\
=& \sum_v C_{vi}\sum_{j=1}^{N/2}\sum_{\lambda, \sigma}C_{\lambda j}C_{\sigma j} (u\lambda|\sigma v)
\end{align*}
$$

## Matrix form and charge density matrix

Now we want to write the Hartree equation $\eqref{hartree_raw}$ and Hartree-Fock equation $\eqref{hf_raw}$ in matrix form. We restate them below:

$$
\begin{align*}
\sum_v (H_{uv} + \sum_{j (\neq i)}\sum_{\lambda, \sigma}C_{\lambda j}C_{\sigma j}(uv|\lambda\sigma))C_{vi} &= E_i \sum_v S_{uv}C_{vi} \\
\sum_v [H_{uv} + \sum_{j=1}^{N/2} (2 \sum_{\lambda, \sigma}C_{\lambda j}C_{\sigma j}(uv|\lambda\sigma) - \sum_{\lambda, \sigma} C_{\lambda j}C_{\sigma j}(u\lambda|\sigma v))]C_{vi} &= E_i \sum_v S_{uv}C_{vi}\\
\forall i = 1, \cdots, N, u = 1, \cdots, K
\end{align*}
$$

We focus on Hartree-Fock equation below. Recall that in the simplest case we got $HC_i = \epsilon_i SC_i \eqref{eq3}$ from $\sum_{v=1}^K H_{uv} C_{vi} = \epsilon_i \sum_{v=1}^K S_{uv} C_{vi}, \forall u = 1, \cdots, K \eqref{simplest_raw}$, $u$ and $v$ are row and column indices respectively. Also recall that in coulomb term $\eqref{E_coulomb_C}$, basis index $u, v$ correspond to the first electron $i$, $\lambda, \sigma$ correspond to the second electron $j$. Let

$$
\begin{align*}
G_{uv} &= \sum_{j=1}^{N/2} (2 \sum_{\lambda, \sigma}C_{\lambda j}C_{\sigma j}(uv|\lambda\sigma) - \sum_{\lambda, \sigma} C_{\lambda j}C_{\sigma j}(u\lambda|\sigma v)) \\
&= \sum_{\lambda, \sigma}\underbrace{(2\sum_{j=1}^{N/2} C_{\lambda j}C_{\sigma j})}_{P_{\lambda\sigma}}(uv|\lambda\sigma) - \frac{1}{2}\sum_{\lambda, \sigma}\underbrace{(2\sum_{j=1}^{N/2} C_{\lambda j}C_{\sigma j})}_{P_{\lambda\sigma}}(u\lambda|\sigma v)
\end{align*}
$$

Here we move the sum operator over all other electrons $\sum_{j=1}^{N/2}$ to the inner of the term, and define

$$
\begin{equation}
P_{\lambda\sigma} = 2\sum_{j=1}^{N/2} C_{\lambda j}C_{\sigma j} \label{P_def}
\end{equation}
$$

(Or in matrix form $P = 2CC^T$ in which $C$ is a $K \times N/2$ matrix. Here $P$ is called *density matrix*. We will discuss the physical meaning of $P$ [in later section](#charge-density-matrix)), thus

$$
\begin{equation}
G_{uv} = \sum_{\lambda, \sigma}P_{\lambda\sigma}(uv|\lambda\sigma) - \frac{1}{2}\sum_{\lambda, \sigma}P_{\lambda\sigma}(u\lambda|\sigma v) \label{G_def}
\end{equation}
$$

Now we can write Hartree-Fock equation in matrix form, similar to $\eqref{eq3}$, that is,

$$
(H + G)C_i = \epsilon_i SC_i
$$

Or more concisely, define *Fock matrix* $F = H + G$ (see (3.154) of [1]), we have 

$$
\begin{equation}
FC_i = \epsilon_i SC_i \label{HF}
\end{equation}
$$

which is also a (generalized) eigen decomposition in which we solve $K$ eigenvalue $\epsilon_1, \cdots, \epsilon_K$ and $K$ elgenvectors $C_1, \cdots, C_K$ for the Fock matrix $F$.

### Self-Consistent Field (SCF)

However, note that we cannot directly solve $\eqref{HF}$ as stated in the simplest case $\eqref{eq3}$. Different from $H$ in $\eqref{eq3}$ which is a known constant, $F$ is dependent on $G$, $G$ is dependent on $P \eqref{G_def}$, $P$ is dependent on $C_1, \cdots, C_{N/2} \eqref{P_def}$. In a word, $F$ is a function of $C$ ($F = F(C)$), but $C$ is just the result that we want to solve. So this becomes a chicken-egg problem. To solve $\eqref{HF}$ we need $C$ to construct Fock matrix $F$, to obtain $C$ we need to solve $\eqref{HF}$, ..., which is a dead lock.

In reality we can try Self-Consistent Field (SCF) method to solve the equation. Simply speaking, we randomly choose a $C_0$ so that we can construct $F$ and solve $\eqref{HF}$ to obtain a new $C_1$. Iterate the process until the new generated $C_i$ is nearly the same as the previous $C_{i-1}$. In implementation we actually use $P$ to replace $C$ in the aforementioned iteration process. That is:

- Choose an initial $P$
- iterate until $\|P - P'\| < \text{threshold}$:
  - Compute Fock matrix $F = H + G(P)$
  - Solve $FC_i = \epsilon_i SC_i \eqref{HF}$ via the procedure introduced in [previous section](#the-simplest-case-both-v_ee-and-antisymmetry-principle-ignored), obtain $K$ elgenvectors $C_1, \cdots, C_K$ whose eigenvalue is in ascending order.
  - Store previous density matrix $P' = P$ and compute new $P = 2CC^T$, in which $C = [C_1, \cdots, C_{N/2}]$ is a $K \times N/2$ matrix consisting of the first $N/2$ eigenvectors.

Unfortunately, SCF is not guaranteed to converge. The selection of initial value and the tricks in iteration process (such as DIIS) is critical to its convergence.

### Charge density matrix

Note that $P_{\lambda\sigma}$ in $\eqref{P_def}$ actually has physical meaning. Remind that $\psi_i(r)$ (orbital) is the wave function of a single electron. Therefore $\rho_i(r) = \psi_i^2(r)$ is the probability density that this electron shows at coodinate $r$, $\int_{r} \psi^2_i(r) dr = 1$. Then

$$
\begin{align*}
\rho_i(r) = \psi_j^2(r) &= (\sum_{\lambda=1}^K C_{\lambda j} \phi_\lambda(r))(\sum_{\sigma=1}^K C_{\sigma j} \phi_\sigma(r)) \\
&= \sum_{\lambda\sigma}C_{\lambda j}C_{\sigma j} \phi_\lambda(r)\phi_\sigma(r)
\end{align*}
$$

Since $K$ basis function $\phi_1(r), \cdots, \phi_K(r)$ are given, the above expression shows that, if we know $K$ variables $C_{1j}, \cdots, C_{Kj}$ then we can get the probability density function of a single orbital $\psi_j^2(r)$.

So how many variables do we need if we want the total density function of all $N$ electrons. That is $\rho(r) = 2\sum_{j=1}^{N/2}\rho_j(r)$ ($\int_{r} \rho(r) dr = N$)? A simple answer is $(N/2) \times K$ matrix of $C$. However,

$$
\begin{align*}
\rho(r) = 2\sum_{j=1}^{N/2}\psi_j^2(r) &= 2\sum_{j=1}^{N/2}\sum_{\lambda\sigma}C_{\lambda j}C_{\sigma j} \phi_\lambda(r)\phi_\sigma(r) \\
&= \sum_{\lambda\sigma}(2\sum_{j=1}^{N/2}C_{\lambda j}C_{\sigma j}) \phi_\lambda(r)\phi_\sigma(r) \\
&= \sum_{\lambda\sigma}P_{\lambda\sigma} \phi_\lambda(r)\phi_\sigma(r)
\end{align*}
$$

So besides $C$, a $K \times K$ matrix $P$ is also sufficient to describe the total density function $\rho(r)$. For this reason we call $P$ the *(charge) density matrix*.

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
3. <https://gitlab.com/LLindenbauer/Hartree-Fock-Tutorial>
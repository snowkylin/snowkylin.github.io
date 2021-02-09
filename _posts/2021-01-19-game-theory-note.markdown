---
layout: post
title:  "Game Theory Note"
author: Snowkylin
categories: game-theory multi-agent-ai
---

* TOC
{:toc}

# Static Game

Strategy form game matrix

|  | Strategies 1 (col player) | Strategies 2 (col player) |
| Strategies 1 (row player, "you") | row player / column player payoff | ... |
| Strategies 2 (row player, "you") | ... | ... |

e.g., the prisoner's dilemma

|  | Don't confess (col player) | confess (col player) |
| Don't confess (row player, "you") | -1 (row), -1 (col) | -10, 0 |
| confess (row player, "you") | 0, -10 | -4, -4 |

- **Strictly dominant strategy for player p**: No matter what strategy the other play choose, player p should choose this strategy
  - e.g., in prisoner's dilemma, 
    - if your partner choose not to confess (see column 1), you should choose confess (0 > -1)
    - if your partner choose to confess (see column 2), you should still choose confess (-4 > -10)
    - Therefore confessing is a strictly dominant strategy for you.
    - Similarly, confessing is also a strictly dominant strategy for the other player.
- **Best response to strategy S**: Assume that the other play choose strategy S, the strategy that has the largest payoff for you is the best response to S.
  - A dominate strategy for player p is a best response to every strategy of the other player (it is all possible that no player, one player and both two players have dominate strategies)
  - If one player has a dominate strategy, the other player can assume that this player will play the dominate strategy.
- **Nash equilibrium**: if player p has a strategy S and player q has a strategy T, we say (S, T) is a Nash equilibrium if S is a best response to T and T is a best response to S.
  - At Nash equilibrium, no one want to *unilaterally* (done only by one player, not both) switch to another strategy.
  - A game can have no Nash equilibrium or multiple Nash equilibrium
- **Game**: 
  - A set of *players* $i \in \\{1, \cdots, N\\}$
  - Each player $i$ have a set of *strategies* $s_i \in S_i$, $s \in S = \Pi_i S_i$ for all players.
  - **Payoff/utility function**: For each combination of strategies (e.g., pair $(s_1, s_2)$ in two-player game), each player $i$ receives a *payoff* $u_i(s_1, s_2)$ (e.g., for pair $(\text{confess}, \text{not confess})$, player 1 may receive payoff 0, $u_1(\text{confess}, \text{not confess}) = 0$ and player 2 may receive payoff -10, $u_2(\text{confess}, \text{not confess}) = -10$). $u_i$ is called the utility function of player $i$.
  - Can be extended to n-player game
- **Pure & Mixed Strategy**: a player $i$ choose a strategy $s$ deterministically or with probability $\sigma_i(s)$  
  - $\Sigma_i$ denotes all the possible probability distribution of $S_i$, $\sigma_i \in \Sigma_i$ for player $i$. $\sigma \in \Sigma = \Pi_i \Sigma_i$ for all players ($\sigma$ is called **mixed strategy profile**).
  - Use *expected value of payoffs* to evaluate other player's strategy (to find the best response of mixed strategy). Extend the payoff function from $u_i(s)$ to $u_i(\sigma) = \Pi_{s \in S} u_i(s)\sigma(s)$
- **$B_i(\sigma_{-i})$: Best Response of player $i$ to other players' strategy $\sigma_{-i}$**
  - For player $i$, given other players' strategy $\sigma_{-i}$ fixed, $B_i(\sigma_{-i}) = \arg\max_{\sigma_i} u_i(\sigma_i, \sigma_{-i})$ is a set of mixed strategy $\sigma_i$ for player $i$ that maximize payoff function $u_i(\sigma_i, \sigma_{-i})$.
- **Nash equilibrium for mixed strategy**
  - A mixed strategy profile $\sigma^* = (\sigma^\ast_1, \cdots, \sigma^\ast_N)$ is a Nash equilibrium if for all $i \in \\{1, \cdots, N\\}$, $\sigma^\ast_i \in B_i(\sigma^\ast_{-i})$.
  - We can write it in a more compact way: denote $B(\sigma) = [B_1(\sigma_{-1}) \times \cdots \times B_N(\sigma_{-N})]$, then a mixed strategy profile $\sigma^\ast = (\sigma^\ast_1, \cdots, \sigma^\ast_N)$ is a Nash equilibrium if $\sigma^\ast \in B(\sigma^\ast)$.
  - **Theorem: Every finite game has a mixed strategy Nash equilibrium**
    - Kakutani's Fixed Point Theorem
- **Pareto optimality**
  - A strategy profile is Pareto optimal if no player can gain higher payoff without sacrificing others' payoffs.
- Social optimality: maximizes the sum of the players‘ payoffs
- Solution as a result of learning
  - Assumption: expects that his opponent’s output in the future will be the same as it is now
  - Asymptotically stable if converges to a particular steady state (a single NE) for all initial state close to it
    - Global stable if from every starting state
    - Not always converge – circling effect

# Extensive Game

Example: 
- player 1 has choice C and D
  - player 2 has choice E (2, 1) / F (3, 0) for C 
  - player 2 has choice G (0, 2) / H (1, 3) for D.

|  | EG | EH | FG | FH |
| C | 2, 1 | 2, 1 | 3, 0 | 3, 0 |
| D | 0, 2 | 1, 3 | 0, 2 | 1, 3 |

- EG ← (E or F) and (G or H)
  - if you choose C, I choose E, if you choose D, I choose G
  - if player 1 have 3 choices C, D and I, then we need to use three letters to denote a strategy of player 2

- **Subgame Perfect (Nash) Equilibrium**
  - A strategy profile is a subgame perfect equilibrium if it is an equilibrium for every subgame
  - subgame perfection will remove noncredible threats

# Potential Game

Use one global function $\Phi(s): S \rightarrow R $ to replace $N$ payoff functions $u_i(s)$ for each player $i$.

$\Phi$ assigns a real value for every $s \in S$

- **ordinal potential function**: $u_i(x, s_{−i}) − u_i(z, s_{−i}) > 0 \Leftrightarrow \Phi(x, s_{−i}) − Φ(z, s_{−i}) > 0, \forall x, z \in S_i$
- **exact potential function**: $u_i(x, s_{−i}) − u_i(z, s_{−i}) = \Phi(x, s_{−i}) − Φ(z, s_{−i}) > 0, \forall x, z \in S_i$
- Every finite ordinal potential game has at least one pure strategy Nash equilibrium.
  - The global maximum of an ordinal potential function is a pure strategy Nash equilibrium
  - there may also be other pure strategy Nash equilibria corresponding to local maxima
- Path / improvement path
- In every finite ordinal potential game, every improvement path is finite.
- Congestion Games
- Every congestion game is a potential game

# Learning Nash Equilibria

2-person **zero-sum** games ($u_1(s_1, s_2) == -u_2(s_1, s_2)$)

| | M (prob: $y_1$) | T (prob: $y_2$) |
| E (prob: $x_1$) | 3, -3 | -1, 1 |
| S (prob: $x_2$) | -2, 2 |  1, -1 |

- Minimax Game: minimize other player's best response (max) payoff
  - For column player, expected payoff $V(M) = -3 x_1 + 2 x_2$, $V(T) = x_1 - x_2$
  - So if $x_1, x_2$ is given, column player will choose the larger one of $V(M)$ and $V(T)$ as best response, his payoff will be $\max(-3 x_1 + 2 x_2, x_1 - x_2)$
  - Row player should choose $x_1$ and $x_2$ so that column player's payoff $\max(-3 x_1 + 2 x_2, x_1 - x_2)$ is minimized, that is, $\arg\min_{x_1, x_2} \max(-3 x_1 + 2 x_2, x_1 - x_2)$
  - Identially, maximize his own payoff $-\max(-3 x_1 + 2 x_2, x_1 - x_2) = \min(3 x_1 - 2 x_2, - x_1 + x_2)$
  - Min/max operation can be represented by linear programming by using an auxilary variable $z$ and constraint all components larger/smaller than $z$.
  - So the linear programming problem is (note that $z$ should be unbounded so we need to add auxilary variable $z'$ so that $z - z'$ is unbounded, $x_1 + x_2 = 1$ is equalty constraint so we need to transit it to two inequality constraint $x_1 + x_2 \leq 1$ and $x_1 + x_2 \geq 1$)
  
    $$
    \begin{align*}
        \max \quad & z \\
        s.t. \quad 3 x_1 - 2 x_2 &\geq z \\
          - x_1 + x_2 &\geq z \\
          x_1 + x_2 &= 1 \\
          x_1, x_2 &\geq 0 \\
        \max \quad & (0, 1, 1, -1, 0, 0)(x_1, x_2, z, z', s_1, s_2)^T \\
        s.t. \quad &\begin{pmatrix}
          3 & -2 & -1 & 1 & -1 & 0 \\ 
          -1 & 1 & -1 & 1 & 0 & -1\\ 
          1 & 1 & 0 & 0 & 0 & 0
        \end{pmatrix}\begin{pmatrix}x_1\\x_2\\z\\z'\\s_1\\s_2\end{pmatrix} = \begin{pmatrix}0\\0\\1\end{pmatrix}\\
        & (x_1, x_2, z, z', s_1, s_2) \geq 0 \\
        \max \quad & (0, 1, 1, -1)(x_1, x_2, z, z')^T \\
        s.t. \quad &\begin{pmatrix}
          3 & -2 & -1 & 1 \\ 
          -1 & 1 & -1 & 1 \\ 
          1 & 1 & 0 & 0 \\
          -1 & -1 & 0 & 0 \\
        \end{pmatrix}\begin{pmatrix}x_1\\x_2\\z\\z'\end{pmatrix} \geq \begin{pmatrix}0\\0\\1\\1\end{pmatrix}\\
        & (x_1, x_2, z, z') \geq 0
    \end{align*}
    $$
  - Similarly, for the column player, we calculate $\arg\min_{y_1, y_2} \max(3 y_1 - y_2, -2 y_1 + y_2)$
  
    $$
    \begin{align*}
        \max \quad & w \\
        s.t. \quad -3 y_1 + y_2 &\geq w \\
          2 y_1 - y_2 &\geq w \\
          y_1 + y_2 &= 1 \\
          y_1, y_2 &\geq 0 \\
        \max \quad & (0, 0, 1) (y_1, y_2, w)^T \\
        s.t. \quad & \begin{pmatrix}
          3 & -1 & 1 \\
          -2 & 1 & 1 \\
          -1 & -1 & 0 \\
          1 & 1 & 0 \\
          -1 & 0 & 0 \\
          - & -1 & 0
        \end{pmatrix} \begin{pmatrix}y_1\\y_2\\w\end{pmatrix} \leq 0\\
        \max \quad & (0, 0, 1, -1) (y_1, y_2, w, w')^T \\
        s.t. \quad & \begin{pmatrix}
          3 & -1 & 1 & -1 \\
          -2 & 1 & 1 & -1 \\
          -1 & -1 & 0 & 0\\
          1 & 1 & 0 & 0\\
        \end{pmatrix} \begin{pmatrix}y_1\\y_2\\w\\w'\end{pmatrix} \leq 0\\
        & (y_1, y_2, w, w') \geq 0
    \end{align*}
    $$
  - Duality
- General-sum game: linear complementarity problem
  - Algorithm to solve LCP: the Lemke–Howson algorithm


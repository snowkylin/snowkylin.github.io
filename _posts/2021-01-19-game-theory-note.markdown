---
layout: post
title:  "Game Theory Note"
author: Snowkylin
categories: game-theory multi-agent-ai
---

* TOC
{:toc}

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
  - Each player $i$ have a set of *strategies* $s_i \in S_i$
  - For each combination of strategies (e.g., pair $(s_1, s_2)$ in two-player game), each player $i$ receives a *payoff* $u_i(s_1, s_2)$ (e.g., for pair $(\text{confess}, \text{not confess})$, player 1 may receive payoff 0, $u_1(\text{confess}, \text{not confess}) = 0$ and player 2 may receive payoff -10, $u_2(\text{confess}, \text{not confess}) = -10$). $u_i$ is called the *utility function* of player $i$.
  - Can be extended to n-player game
- **Pure & Mixed Strategy**: a player choose a strategy deterministically or with probability p 
  - Use *expected value of payoffs* to evalueate other player's strategy (to find the best response of mixed strategy)
  - **Theorem: Every finite game has a mixed strategy Nash equilibrium**


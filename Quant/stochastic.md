# Mistakes collection: Stochastic process and stochastic calculus

> From: A practical guide to quantitative finance interviews 绿皮书 Zhou Xinfeng

* **Markove chain**: Once the current state is known, past history has no bearing on the future
    - **Transition matrix / graph** 
    - Equation for **absorption probability**

        The probability to reach a specific absorbing state:
        * For all absorbing states i != s: a_s = 1, a_i = 0
        * For all transition states: a_i = sum_{j=1}^{m} a_j p_ij

    - Equation for **expected time to absorption**
        + For absorbtion state: u_i = 0
        + For transient state: u_i = 1 + sum_{j=1}^{m} p_ij u_i, 1 means thaking 1 step to next statae
* **Martingale**： the conditional expected value of future Z_m is the current value Z_n.
    - **Stopping rule**: whether to stop at n depends only on X_1, X_2, ... X_n. No look ahead.
    - **Wald's equality**: For IID X, S_N = X_1 + X_2 + X_3 + ... + X_n; E[S_N] = E[X]E[N]
    - If S_N is martingale, then (S_N)^2 is also a martingale
* **Dynamic programming**：
* **Brownian Motion and Srochastic calculus**：


### First time go through (21/05/2020)

Markov:
1. Gambler's ruin game ** P107: graph, matrix 
1. Dice question *** P108: conditional probability & graph
1. Coin triplets **** P109: classic
2. Hard: Color ball P113:

Martingale:
1. Drunk man **** P116: classic
2. Dice game *** P117: Wald's equality
3. Hard: Ticket line
4. Hard: Coin sequence

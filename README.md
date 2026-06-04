# Reinforcement Learning for Option Hedging (Kolm & Ritter Inspired)

## Overview

This project implements a reinforcement learning approach to dynamic option hedging inspired by the work of Kolm and Ritter on optimal hedging under market frictions.

Instead of replicating the classical Black–Scholes delta hedge, the agent learns a trading policy that balances:

- Hedging performance,
- Inventory risk,
- Transaction costs.

The objective is to maximize a risk-adjusted wealth criterion through sequential trading decisions.

---

## Model

The environment consists of:

- A single underlying asset following a Geometric Brownian Motion (GBM),
- A European call option written on the underlying,
- Discrete hedging dates,
- A quadratic utility objective.

At each time step, the agent observes the state

$
(S_t,\tau_t,n_t)
$

where:

- \(S_t\) is the underlying price,
- \(\tau_t\) is the remaining time to maturity,
- \(n_t\) is the current stock inventory.

The action corresponds to the number of shares traded at each hedging date.

---

## Reward Function

The wealth increment is defined as

\[
\Delta W_t
=
n_t(S_{t+1}-S_t)
-
\text{Cost}(a_t)
+
\Delta V_t,
\]

where:

- \(n_t\) is the current inventory,
- \(a_t\) is the trade executed,
- \(\Delta V_t\) is the option price variation.

The agent maximizes the risk-adjusted reward

\[
R_t
=
\Delta W_t
-
\frac{\kappa}{2}
(\Delta W_t)^2,
\]

where \(\kappa\) controls risk aversion.

---

## Learning Algorithm

The implementation uses:

- Fitted Q Iteration (FQI),
- Extra Trees Regressors as function approximators,
- ε-greedy exploration,
- Multiple training batches with decreasing exploration rates.

The Q-function is approximated as

\[
Q(S_t,\tau_t,n_t,a_t),
\]

and updated according to the Bellman recursion

\[
Q_t
=
R_t
+
\gamma
Q(S_{t+1},\tau_{t+1},n_{t+1},a_{t+1}),
\]

where \(\gamma\) is the discount factor.

---

## Training Procedure

1. Generate simulated price paths under GBM.
2. Collect state-action-reward samples.
3. Fit an Extra Trees model to approximate the Q-function.
4. Use the learned model to estimate continuation values.
5. Repeat over several batches while gradually reducing exploration.

---

## Outputs

The code produces:

- Learned inventory trajectories,
- Comparison with Black–Scholes delta hedging,
- Underlying price evolution,
- Transaction cost evolution,
- Cumulative option PnL,
- Cumulative stock PnL,
- Total hedging PnL.

Example visualizations include:

- Learned stock position vs. theoretical delta hedge,
- Option and stock PnL decomposition,
- Cumulative hedging performance.

---

## Reference

This implementation is inspired by:

> Kolm, P. N., & Ritter, G. (2019).
> *Modern Perspectives on Reinforcement Learning in Finance.*

The project explores how reinforcement learning can be used to learn dynamic hedging policies that account for market frictions and risk preferences.

---

## Future Improvements

Potential extensions include:

- Explicit proportional and nonlinear transaction costs,
- Stochastic volatility models (Heston, SABR),
- Deep Q-Networks (DQN),
- Actor-Critic methods,
- Continuous action spaces,
- Multi-asset hedging,
- Comparison against classical delta and utility-based hedging strategies.

---

## Disclaimer

This project is intended for educational and research purposes. It is not an exact reproduction of the original Kolm & Ritter framework but rather a simplified implementation inspired by their reinforcement learning approach to option hedging.

# Warlock — `lstm-long-only`

## Motive

`main` trains a recurrent (LSTM) PPO policy with an unrestricted long/short action space. Earlier long/short runs showed deeply negative Sharpe ratios and frequently tripped the drawdown circuit breaker. The hypothesis tested on this branch: an unrestricted action space gives the policy too much surface area to exploit the reward function (e.g. leveraged or oscillating short positions) instead of learning a coherent strategy. This branch removes shorting to test whether constraining the action space reduces that reward-hacking behaviour.

## Difference from `main`

- **Policy:** same as `main` — `MlpLstmPolicy` (recurrent PPO, LSTM-based).
- **Action space:** net position weight is clamped to `[0.0, 1.0]` (long-only, no short leg, no leverage past 1x), instead of `main`'s `[-1.0, 1.0]`.
- **Assets:** single-asset (BTC/USDT), same as `main`.
- Reward shaping, portfolio simulator, and Optuna search harness are otherwise unchanged from `main`.

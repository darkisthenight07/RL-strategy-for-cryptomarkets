# Warlock — `mlp-long-only`

## Motive

`main` uses a recurrent (LSTM) policy, which carries temporal context across steps in its own hidden state. This branch tests whether that recurrence is actually necessary, by swapping in a plain feedforward (MLP) policy and combining it with a long-only action space — isolating two variables at once: does removing the LSTM hurt performance, and does removing shorting reduce the reward-hacking/circuit-breaker behaviour seen on `main`.

## Difference from `main`

- **Policy:** `MlpPolicy` (non-recurrent, feedforward), instead of `main`'s `MlpLstmPolicy`. No hidden state carried across steps.
- **Action space:** net position weight is clamped to `[0.0, 1.0]` (long-only, no short leg), instead of `main`'s `[-1.0, 1.0]`.
- **Assets:** single-asset (BTC/USDT), same as `main`.
- Reward shaping and portfolio simulator are otherwise unchanged from `main`.

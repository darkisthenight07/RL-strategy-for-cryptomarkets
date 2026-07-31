# Warlock — `mlp-long-short`

## Motive

This branch isolates the effect of the policy architecture alone. It keeps `main`'s full long/short action space unchanged, and only swaps the recurrent (LSTM) policy for a plain feedforward (MLP) policy. The goal is a clean architecture comparison against `main` — same action space, same task — to see whether the LSTM's temporal memory is actually contributing to performance, without also changing what the agent is allowed to do.

## Difference from `main`

- **Policy:** `MlpPolicy` (non-recurrent, feedforward), instead of `main`'s `MlpLstmPolicy`. No hidden state carried across steps.
- **Action space:** unrestricted long/short, `[-1.0, 1.0]` — same as `main`.
- **Assets:** single-asset (BTC/USDT), same as `main`.
- Reward shaping and portfolio simulator are otherwise unchanged from `main`.

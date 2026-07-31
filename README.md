# Warlock — `multiasset`

## Motive

`main` trains and trades a single asset (BTC/USDT). This branch tests whether the agent can learn a useful policy across multiple correlated assets simultaneously, sharing one model instead of training separate single-asset agents. It extends the environment to a second asset (ETH/USDT) and restructures the observation into a per-asset market state plus a shared portfolio vector, so the policy can learn cross-asset allocation rather than a single-instrument position size.

## Difference from `main`

- **Policy:** `MultiInputLstmPolicy`, instead of `main`'s `MlpLstmPolicy` — required because the environment now emits a `Dict` observation (`{"market": (n_assets, n_features), "portfolio": (k,)}`) instead of a single flat vector.
- **Assets:** BTC/USDT + ETH/USDT, instead of `main`'s single-asset (BTC/USDT) setup. Assets are aligned on a shared timestamp index (inner join).
- **Feature pipeline:** per-asset train-fit normalization applied independently to each symbol before merging, plus a dedicated `MultiAssetFeaturesExtractor` (shared per-asset encoder) to process the multi-asset observation.
- **Action space:** unrestricted long/short, `[-1.0, 1.0]` per asset — same as `main`.

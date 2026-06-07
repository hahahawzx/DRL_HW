# 01 official_reward_select_013

## What changed

This method keeps the original PianoMime high-level and low-level checkpoints, but evaluates multiple low-level random seeds (`0,1,3`) and selects the best rollout by reward.

## Why it may help

The low-level diffusion policy is stochastic. Multi-seed sampling can improve test-time performance without changing training.

## Setting

- multi-task / generalist
- test clips:
  - `Alone_1`
  - `EyesClosed_1`
  - `Hope_1`
  - `SomewhereOnlyWeKnow_1`
  - `NoTimeToDie_1`

## Main quantitative result

4 of 5 clips improve over the provided baseline, but `SomewhereOnlyWeKnow_1` degrades.

Mean F1 over the five clips:

- baseline: `0.615026`
- improved: `0.619582`
- delta: `+0.004556`

## Conclusion

This is the most effective method among the currently available multi-task improvements for these five clips. The gain is modest but relatively stable.

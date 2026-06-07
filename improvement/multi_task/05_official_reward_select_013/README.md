# Reward-based Rollout Selection

## What changed

This method does not train a new model. It keeps the official PianoMime high-level and low-level checkpoints fixed, then uses the stochasticity of the low-level diffusion policy at test time.

For each clip, multiple rollout seeds are evaluated and the better rollout is selected by reward / F1 proxy.

## Motivation

Diffusion policies generate actions by sampling. Even with the same high-level trajectory and the same checkpoint, different low-level random seeds can produce different contact timing and missed-key patterns. Best-of-N sampling uses this stochasticity as a simple inference-time improvement.

## Forester_1 best-of-N evidence

With the high-level trajectory fixed to the official seed-0 trajectory:

| Low-level seed | Precision | Recall | F1 |
|---:|---:|---:|---:|
| 0 | 0.8049 | 0.7107 | 0.6731 |
| 1 | 0.8011 | 0.7297 | 0.6914 |
| 3 | 0.7998 | 0.7265 | 0.6905 |
| 6 | 0.7837 | 0.7311 | 0.6889 |

Best Forester_1 result:

```text
best low-level seed = 1
F1 = 0.6914
delta F1 vs official baseline = +0.0183
```

Increasing low-level denoising steps from 50 to 75 did not help; it reduced F1 to `0.6629`, likely because piano contact timing is sensitive.

## Twelve-song reward selection

The same idea was extended to 12 unseen songs by selecting among multiple official rollout seeds.

```text
official seed0 mean F1 = 0.6095
reward_select_0_1_3_4_6 mean F1 = 0.6165
delta F1 = +0.0070
```

## Five-clip result in this folder

The reader-facing five-clip benchmark reports:

```text
baseline mean F1 = 0.615026
improved mean F1 = 0.619582
delta F1        = +0.004556
```

## Conclusion

This is a test-time generative inference trick, not a training improvement. It is useful because it improves average F1 without changing model weights, but it should not be described as a new trained generalist policy.

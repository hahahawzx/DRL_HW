# Multi-task Improvement Summary

All generalist-policy methods are evaluated on the same five unseen/test clips:

```text
Alone_1
EyesClosed_1
Hope_1
SomewhereOnlyWeKnow_1
NoTimeToDie_1
```

The baseline generalist mean F1 on these clips is `0.615026`.

## Compared Methods

### Smooth Diffusion Objective

Fine-tunes the low-level diffusion policy with denoised action derivative matching.

- mean F1: `0.625574`
- delta F1: `+0.010548`

This is the strongest average improvement among the collected generalist-policy methods.

### A+C Recovery

Uses a two-stage recovery method with teacher regularization.

- mean F1: `0.609421`
- delta F1: `-0.005605`

This method is not a stable improvement on the selected five-clip benchmark.

### RP1M Low-level Replacement

Replaces the official low-level checkpoint with an RP1M-derived low-level policy.

- mean F1: `0.595610`
- delta F1: `-0.019416`

This method improves only `Hope_1` and degrades the other four clips.

### Goal Representation

Fine-tunes the low-level diffusion policy with an expanded goal side-channel representation.

- mean F1: `0.623506`
- delta F1: `+0.008480`

This is the second strongest average improvement and is especially helpful on `EyesClosed_1`.

### Reward-based Rollout Selection

Keeps the original PianoMime checkpoints but samples multiple rollouts and selects the best one by reward.

- mean F1: `0.619582`
- delta F1: `+0.004556`

This method gives a modest positive test-time improvement, but still degrades `SomewhereOnlyWeKnow_1`.

## Conclusion

Smooth Diffusion Objective is the best current generalist-policy improvement by average F1. Goal Representation is also positive and close behind. A+C Recovery, RP1M Low-level Replacement, and Reward-based Rollout Selection are useful comparison points but are not stronger than these two on the selected five clips.

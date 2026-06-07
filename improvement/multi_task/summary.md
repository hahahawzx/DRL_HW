# Multi-task Improvement Summary

All multi-task methods are evaluated on the same five unseen/test clips:

```text
Alone_1
EyesClosed_1
Hope_1
SomewhereOnlyWeKnow_1
NoTimeToDie_1
```

The baseline generalist mean F1 on these clips is `0.615026`.

## Compared Methods

### `01_smooth_diffusion_objective`

Fine-tunes the low-level diffusion policy with denoised action derivative matching.

- mean F1: `0.625574`
- delta F1: `+0.010548`

This is the strongest average improvement among the currently collected multi-task methods.

### `02_ac_recover100_teacher0005_seed0`

Uses an A+C two-stage recovery method with teacher regularization.

- mean F1: `0.609421`
- delta F1: `-0.005605`

This method is not a stable improvement on the selected five-clip benchmark.

### `03_rp1m_pointmajor_linearq_seed3`

Replaces the official low-level checkpoint with an RP1M-derived low-level model.

- mean F1: `0.595610`
- delta F1: `-0.019416`

This method improves only `Hope_1` and degrades the other four clips.

### `04_goal_representation`

Fine-tunes the low-level diffusion policy with an expanded goal side-channel representation.

- mean F1: `0.623506`
- delta F1: `+0.008480`

This is the second strongest average improvement and is especially helpful on `EyesClosed_1`.

### `05_official_reward_select_013`

Keeps the original PianoMime checkpoints but evaluates multiple low-level seeds and selects the best rollout by reward.

- mean F1: `0.619582`
- delta F1: `+0.004556`

This method gives a modest positive test-time improvement, but still degrades `SomewhereOnlyWeKnow_1`.

## Conclusion

`01_smooth_diffusion_objective` is the best current multi-task improvement by average F1. `04_goal_representation` is also positive and close behind. The remaining methods are useful comparison points but are not stronger than these two on the selected five clips.

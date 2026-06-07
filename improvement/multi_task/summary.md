# Multi-task Improvement Summary

`01_smooth_diffusion_objective` fine-tunes the low-level diffusion policy with denoised action derivative matching.

On the five selected unseen/test clips, the baseline generalist mean F1 is `0.615026` and this method reaches `0.625574`, for a delta of `+0.010548`.

## Additional compared methods on the same five clips

### `01_official_reward_select_013`

This method keeps the original PianoMime checkpoints but evaluates multiple low-level seeds and selects the best rollout by reward.

- baseline mean F1: `0.615026`
- improved mean F1: `0.619582`
- delta: `+0.004556`

It improves 4 of the 5 selected clips, but still loses on `SomewhereOnlyWeKnow_1`.

### `02_ac_recover100_teacher0005_seed0`

This is the A+C two-stage recovery method with teacher regularization.

- baseline mean F1: `0.615026`
- improved mean F1: `0.609421`
- delta: `-0.005605`

It helps on some clips but is not a stable improvement on the selected five-song subset.

### `03_rp1m_pointmajor_linearq_seed3`

This method replaces the official low-level checkpoint with the RP1M-derived low-level model.

- baseline mean F1: `0.615026`
- improved mean F1: `0.595610`
- delta: `-0.019416`

It improves only `Hope_1` and degrades the other four selected clips.

## Practical conclusion

Among the currently compared methods for the five selected test clips, `01_smooth_diffusion_objective` remains the strongest average improvement. `official_reward_select_013` is a modest but still positive test-time improvement. The A+C recovery and RP1M low-level replacement are not effective on this five-clip benchmark slice.

# Smooth Diffusion Objective

## What changed

This method fine-tunes the low-level diffusion policy from the original PianoMime checkpoint with an additional denoised-trajectory objective. It modifies the training loss only; it is not a high-level policy change, inference sampler change, or rollout post-processing method.

The baseline low-level diffusion loss predicts denoising noise. For each noisy action chunk, we reconstruct the clean action estimate from the predicted noise:

```text
a_hat_0 = (a_noisy - sqrt(1 - alpha_t) * epsilon_pred) / sqrt(alpha_t)
```

The extra objective is applied only on low-noise timesteps, where `a_hat_0` is close enough to the final denoised action to make trajectory supervision meaningful. The implementation uses `smooth_timestep_max = 30` with linear low-noise weighting.

The reported training loss is:

```text
L =
  L_noise
  + 1e-2 * L_delta
  + 5e-3 * L_jerk
  + 1e-3 * L_abs_jerk
  + 1e-3 * L_x0

L_delta    = ||Delta a_hat_0 - Delta a_demo||_2^2
L_jerk     = ||Delta^2 a_hat_0 - Delta^2 a_demo||_2^2
L_abs_jerk = ||Delta^2 a_hat_0||_2^2
L_x0       = ||a_hat_0 - a_demo||_2^2
```

Actions are split into left-hand and right-hand groups when computing the trajectory terms. The current run uses uniform action weights.

## Motivation

The generalist policy must play unseen clips with accurate timing while keeping hand actions temporally coherent. A note sequence can be mostly correct while the low-level action trajectory still contains abrupt changes. The smooth diffusion objective is intended to make denoised action chunks follow the demonstration's local motion pattern.

This is different from simple action smoothing: direct low-pass smoothing was tested separately and reduced F1 because it delayed contact timing.

## Evaluation

Evaluation uses the same five unseen clips as the generalist baseline:

```text
Alone_1
EyesClosed_1
Hope_1
SomewhereOnlyWeKnow_1
NoTimeToDie_1
```

Result:

```text
baseline mean F1 = 0.615026
improved mean F1 = 0.625574
delta F1        = +0.010548
```

## Files

- `metrics.csv`: cleaned per-clip Precision, Recall, and F1.
- `comparison_to_baseline.csv`: per-clip F1 comparison against the baseline generalist policy.
- `figures/f1_comparison.png`: baseline vs improved F1 comparison.
- `videos/`: per-clip performance videos.
- `run_config.md`: compact reproduction details.

## Conclusion

This is the strongest average improvement on the five-clip generalist benchmark currently stored in this folder.

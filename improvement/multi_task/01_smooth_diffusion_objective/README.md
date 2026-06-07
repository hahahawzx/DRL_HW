# Smooth Diffusion Objective

## What changed

This method fine-tunes the low-level diffusion policy from the original PianoMime checkpoint with an additional smoothness objective on the denoised action trajectory.

The baseline low-level diffusion loss predicts denoising noise. We add a derivative-matching term so the predicted clean action chunk has smoother local dynamics:

```text
L = L_diff + beta * L_smooth
L_smooth compares action differences between predicted actions and demonstration actions
```

## Motivation

The generalist policy must play unseen clips with accurate timing while keeping hand actions temporally coherent. A note sequence can be mostly correct while the low-level action trajectory still contains abrupt changes. The smooth diffusion objective is intended to reduce these local action inconsistencies without applying a hard test-time low-pass filter.

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

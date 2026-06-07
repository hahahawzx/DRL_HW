# Smooth Diffusion Objective

This folder contains the multi-task/generalist improvement result for the smooth diffusion objective.

Method summary: the low-level diffusion policy is fine-tuned from the original PianoMime checkpoint with an additional denoised action trajectory objective. The added terms match the first- and second-order action differences of the predicted clean action chunk to the demonstration action chunk at low-noise diffusion timesteps.

Evaluation uses the same five unseen/test clips as the generalist baseline:

```text
Alone_1
EyesClosed_1
Hope_1
SomewhereOnlyWeKnow_1
NoTimeToDie_1
```

Mean F1 over the five clips: `0.625574`. Baseline mean F1: `0.615026`. Delta F1: `+0.010548`.

Files:

- `metrics.csv`: cleaned per-clip Precision, Recall, and F1.
- `comparison_to_baseline.csv`: per-clip F1 comparison against the baseline generalist policy.
- `summary.csv`: mean result for this method.
- `figures/f1_comparison.png`: baseline vs improved F1 comparison.
- `videos/`: per-clip performance videos.
- `run_config.md`: compact reproduction details.

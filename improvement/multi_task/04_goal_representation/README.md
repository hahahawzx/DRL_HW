# Goal Side-channel Representation

This folder contains the multi-task/generalist improvement result for the goal side-channel representation method.

Method summary: the low-level diffusion policy is fine-tuned with an expanded conditioning vector. The original observation conditioning is augmented with a compact goal side-channel, so the policy receives additional future-goal information while predicting the low-level action chunk.

Evaluation uses the same five unseen/test clips as the generalist baseline:

```text
Alone_1
EyesClosed_1
Hope_1
SomewhereOnlyWeKnow_1
NoTimeToDie_1
```

Mean F1 over the five clips: `0.623506`. Baseline mean F1: `0.615026`. Delta F1: `+0.008480`.

Files:

- `metrics.csv`: cleaned per-clip Precision, Recall, and F1.
- `comparison_to_baseline.csv`: per-clip F1 comparison against the baseline generalist policy.
- `summary.csv`: mean result for this method.
- `figures/f1_comparison.png`: baseline vs improved F1 comparison.
- `videos/`: per-clip performance videos.
- `run_config.md`: compact reproduction details.

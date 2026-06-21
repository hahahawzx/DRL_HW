# Goal Side-channel Representation

## What changed

This method fine-tunes the low-level diffusion policy with an expanded conditioning vector. The original low-level policy conditions on proprioception, piano/key state, and high-level fingertip trajectory. We add a compact future-goal side-channel so the low-level policy receives more explicit information about upcoming note targets.

## Motivation

In generalist piano playing, similar robot states can require different actions depending on the target note pattern. Weak goal conditioning can make the policy ambiguous: it may move plausibly but press late, miss onsets, or choose a locally reasonable action for the wrong upcoming notes. The goal side-channel is intended to make the low-level policy more goal-aware.

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
improved mean F1 = 0.623506
delta F1        = +0.008480
```

This method improves the mean five-clip F1 and is especially helpful on `EyesClosed_1`.

## Files

- `metrics.csv`: cleaned per-clip Precision, Recall, and F1.
- `comparison_to_baseline.csv`: per-clip F1 comparison against the baseline generalist policy.
- `figures/f1_comparison.png`: baseline vs improved F1 comparison.
- `videos/`: per-clip performance videos.
- `run_config.md`: compact reproduction details.

## Conclusion

Goal-side conditioning is a positive generalist-policy modification, though it is slightly weaker than the smooth diffusion objective on the current five-clip benchmark.

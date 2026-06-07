# RP1M Recovery with Teacher Regularization

## What changed

This method tries a two-stage RP1M-to-PianoMime low-level training path:

1. A: pretrain / fine-tune the low-level diffusion policy with RP1M-derived low-level data to increase state-action coverage.
2. C: recover on the original PianoMime low-level dataset to reduce distribution shift.
3. Teacher regularization: keep the student close to the frozen official PianoMime low-level checkpoint during recovery.

Training loss:

```text
loss = diffusion_noise_mse + lambda * mse(student_noise_pred, teacher_noise_pred)
```

## Motivation

RP1M provides broader piano-playing trajectories, but its data schema and state distribution do not exactly match PianoMime. Direct replacement can drift away from the official low-level policy. Recovery on original data and teacher regularization are intended to keep the benefit of RP1M coverage while reducing this drift.

## Setting

```text
stage-1 checkpoint = RP1M point-major + learned linear-q low-level checkpoint
recovery dataset = pianomime/dataset_ll.zarr
teacher checkpoint = checkpoint_low_level.ckpt
teacher_lambda = 0.005
```

The reader-facing benchmark uses five unseen clips:

```text
Alone_1
EyesClosed_1
Hope_1
SomewhereOnlyWeKnow_1
NoTimeToDie_1
```

## Result

Five-clip result in this folder:

```text
baseline mean F1 = 0.615026
improved mean F1 = 0.609421
delta F1        = -0.005605
```

Broader Forester_1 experiments show that this idea can help under fixed-seed comparison:

```text
official baseline F1                 = 0.6731
recover50 + teacher0.005, seed1 F1   = 0.6897
recover100 + teacher0.005, seed3 F1  = 0.6974
recover100 without teacher, seed1 F1 = 0.6518
```

## Conclusion

Teacher regularization is useful, but the current RP1M recovery setting is not a stable five-clip improvement. It should be reported as a meaningful training attempt and diagnostic result, not as the final strongest generalist method.

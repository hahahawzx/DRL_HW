# Combined Single-task Improvement

## Setting

- Task: single-task specialist policy
- Clip: `Happy_8`
- Outcome: final selected single-task method

## Method

This is the only retained single-task improvement. It combines three changes:

- Onset-aware reward: add extra reward on frames where a note first needs to be pressed. This makes the policy pay more attention to note-onset timing, reducing delayed key presses and missed notes. The main expected F1 benefit is higher recall.
- Light smoothness regularization: add small penalties for abrupt residual/action changes, including residual magnitude penalty, residual delta penalty, action smoothness penalty, and qvel/qacc-style penalties. The coefficients are intentionally small to reduce finger jitter and unnatural large motions without making the policy too conservative.
- Residual factor calibration: sweep the execution-time residual scale and choose a suitable residual factor around `0.0205` or `0.03`, depending on the checkpoint. This avoids residuals that are too large and cause wrong key presses, as well as residuals that are too small and miss notes.

## Result

```text
precision = 1.000000
recall    = 0.889484
f1        = 0.918715
delta_f1  = +0.046265
```

The final single-task F1 is `0.9187`. It improves over the released `Happy_8` single-song rollout baseline while keeping precision at `1.000000`.

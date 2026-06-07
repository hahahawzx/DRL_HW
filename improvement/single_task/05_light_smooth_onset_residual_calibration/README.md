# Light Smoothness + Onset Reward + Residual Calibration

## Setting

- Task: single-task specialist policy
- Clip: `Happy_8`
- Outcome: final selected method

## Attempt

This final method combines light smoothness regularization, onset-aware reward shaping, and residual action scale calibration. The smoothness term is kept light to avoid the recall drop seen in the heavy-smoothness failure case.

## Result

```text
precision = 1.000000
recall    = 0.889484
f1        = 0.918715
delta_f1  = +0.046265
```

This is the selected single-task improvement. It improves over the released `Happy_8` single-song rollout baseline while keeping precision at `1.000000`.

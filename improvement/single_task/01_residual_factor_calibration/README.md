# Residual Factor Calibration

## Setting

- Task: single-task specialist policy
- Clip: `Happy_8`
- Outcome: useful diagnostic

## Attempt

This attempt keeps the trained PPO checkpoint fixed and changes the residual action scale at evaluation time. The goal is to test whether the residual correction is too weak or too strong.

## Result

```text
precision = 0.814019
recall    = 0.558166
f1        = 0.573780
delta_f1  = +0.014241
```

This improves over the short PPO baseline, but the gain is limited. It suggests that residual scaling matters, but calibration alone is not enough.

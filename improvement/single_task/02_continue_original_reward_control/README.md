# Continue Original Reward Control

## Setting

- Task: single-task specialist policy
- Clip: `Happy_8`
- Outcome: control run

## Attempt

This control continues PPO training with the original reward and the same extra training budget, without adding the proposed reward changes. It tests whether improvement comes only from more optimization steps.

## Result

```text
precision = 0.838281
recall    = 0.548723
f1        = 0.570456
delta_f1  = +0.010917
```

The control gives only a small gain, so the final improvement is not explained by extra PPO updates alone.

# Light Smoothness + Onset Reward

## Setting

- Task: single-task specialist policy
- Clip: `Happy_8`
- Outcome: positive intermediate attempt

## Attempt

This attempt uses lighter smoothness regularization and adds onset-aware reward shaping. The goal is to improve motion stability while preserving note timing around note starts.

## Result

```text
precision = 0.852604
recall    = 0.567411
f1        = 0.582861
delta_f1  = +0.023322
```

This improves over the short PPO baseline and shows that smoothness can help when the penalty is not too strong.

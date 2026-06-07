# Heavy Smoothness Penalty

## Setting

- Task: single-task specialist policy
- Clip: `Happy_8`
- Outcome: failed attempt

## Attempt

This attempt adds stronger smoothness penalties to discourage rapid action changes. The intended benefit is smoother motion, but the penalty is too strong for this timing-sensitive piano task.

## Result

```text
precision = 0.798698
recall    = 0.522489
f1        = 0.543064
delta_f1  = -0.016476
```

This attempt fails because recall drops. The policy becomes too conservative and misses notes.

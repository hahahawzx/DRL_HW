# Single-task Improvement Summary

The single-task improvement is evaluated on `Happy_8`. We treat the final result as one combined method rather than several independent optimizations.

## Method

The final method combines three changes:

- Onset-aware reward: add extra reward on frames where a note first needs to be pressed. This makes the policy pay more attention to note-onset timing, reducing delayed key presses and missed notes. The main expected F1 benefit is higher recall.
- Light smoothness regularization: add small penalties for abrupt residual/action changes, including residual magnitude penalty, residual delta penalty, action smoothness penalty, and qvel/qacc-style penalties. The coefficients are intentionally small to reduce finger jitter and unnatural large motions without making the policy too conservative.
- Residual factor calibration: sweep the execution-time residual scale and choose a suitable residual factor around `0.0205` or `0.03`, depending on the checkpoint. This avoids residuals that are too large and cause wrong key presses, as well as residuals that are too small and miss notes.

## Result

| Clip | Precision | Recall | F1 | Baseline F1 | Delta F1 |
|---|---:|---:|---:|---:|---:|
| `Happy_8` | `1.000000` | `0.889484` | `0.918715` | `0.872450` | `+0.046265` |

The final single-task F1 is `0.9187`.

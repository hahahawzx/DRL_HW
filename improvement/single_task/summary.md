# Single-task Improvement Summary

All single-task attempts here use `Happy_8` as the target clip. We keep both successful and failed attempts because the failed settings help explain why the final method was chosen.

The first four attempts are short-run ablations compared against the short PPO baseline F1 `0.559539`. The final selected method is reported against the released `Happy_8` single-song rollout baseline F1 `0.872450`, which is the cleaner comparison for the final project result.

## Attempts

| Folder | Attempt | F1 | Delta F1 | Outcome |
|---|---|---:|---:|---|
| `01_residual_factor_calibration` | residual action scale calibration | `0.573780` | `+0.014241` | useful diagnostic |
| `02_continue_original_reward_control` | continue PPO with original reward | `0.570456` | `+0.010917` | control run |
| `03_heavy_smoothness_penalty_failed` | heavy smoothness penalties | `0.543064` | `-0.016476` | failed |
| `04_light_smooth_onset` | light smoothness + onset reward | `0.582861` | `+0.023322` | positive intermediate |
| `05_light_smooth_onset_residual_calibration` | light smoothness + onset reward + residual calibration | `0.918715` | `+0.046265` | final selected method |

## Conclusion

Heavy smoothness regularization hurts recall and lowers F1, so the final method uses light smoothness instead. The best result comes from combining light smoothness, onset-aware reward shaping, and residual-action calibration.

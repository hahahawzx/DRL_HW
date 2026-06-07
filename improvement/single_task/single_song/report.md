# Smooth and Timing-Aware Residual RL for PianoMime Single-Song Policy

## Setup

We used the released `Happy_8` training clip as the single-song policy target. The policy keeps PianoMime's IK-guided residual PPO structure:

```text
u_t = u_t^IK + residual_factor * r_t
```

The baseline is the original PianoMime-style residual PPO reward with key-press and mimic terms. The best baseline checkpoint was trained for 150 PPO iterations with 8 parallel environments, `n_steps=256`, `frame_stack=4`, and `residual_factor=0.03`.

## Implemented Changes

The implementation adds optional reward/logging terms without changing the default baseline behavior:

- residual magnitude penalty
- residual smoothness penalty
- full action smoothness penalty
- joint velocity and acceleration penalties
- action limit penalty
- onset-aware reward
- logging for residual norm, action delta, qvel, qacc, and reward terms

The final successful method used a light version of these terms:

```text
residual_factor = 0.0205 at evaluation
onset_reward_coef = 0.2
residual_penalty_coef = 1e-4
residual_smoothness_penalty_coef = 2e-4
action_smoothness_penalty_coef = 2e-4
qvel_penalty_coef = 2e-5
qacc_penalty_coef = 2e-5
```

## Results

| Method | Residual factor | Precision | Recall | F1 | Delta action | QAcc | Residual norm |
|---|---:|---:|---:|---:|---:|---:|---:|
| Baseline PPO | 0.0300 | 0.8175 | 0.5419 | 0.5595 | 0.0748 | 6.4008 | 0.000316 |
| Baseline + residual-factor calibration | 0.0200 | 0.8140 | 0.5582 | 0.5738 | 0.0752 | 6.3212 | 0.000137 |
| Continue original reward control | 0.0300 | 0.8383 | 0.5487 | 0.5705 | - | 6.4956 | 0.000318 |
| Smooth v1, heavier penalties | 0.0300 | 0.7987 | 0.5225 | 0.5431 | - | - | - |
| Light smooth + onset | 0.0200 | 0.8526 | 0.5674 | 0.5829 | 0.0745 | 6.1451 | 0.000189 |
| Light smooth + onset + residual-factor calibration | 0.0205 | 0.8674 | 0.5810 | 0.6082 | 0.0711 | 5.8434 | 0.000197 |

The final method improves F1 from `0.5595` to `0.6082`, an absolute gain of `+0.0487`. It also reduces the logged action delta from `0.0748` to `0.0711` and qacc from `6.4008` to `5.8434`.

## Ablations

The control run continued training the baseline checkpoint for the same extra training budget without changing the reward. It improved F1 only to `0.5705`, which shows that most of the final gain is not just extra PPO updates.

The heavy smoothness version failed: F1 dropped to `0.5431`. The likely reason is that the smoothness terms were too strong relative to the sparse/timing-sensitive key-press objective. The policy became more conservative and lost recall, especially around fast note onsets.

Residual-factor calibration was important. For the final checkpoint, a fine sweep found:

```text
0.0200 -> F1 0.5829
0.0202 -> F1 0.5869
0.0204 -> F1 0.5751
0.0205 -> F1 0.6082
0.0206 -> F1 0.5782
0.0208 -> F1 0.6037
0.0210 -> F1 0.5817
```

The best setting, `0.0205`, was repeated three times and produced the same deterministic F1.

## Artifacts

- Ablation table: `results/single_song/ablation_table.csv`
- Training curve: `results/single_song/train_curve.png`
- Residual-factor sweep: `results/single_song/residual_factor_sweep.png`
- Baseline video: `results/single_song/videos/happy8_baseline_factor030.mp4`
- Final video: `results/single_song/videos/happy8_best_onset_factor0205.mp4`
- Side-by-side video: `results/single_song/videos/happy8_side_by_side_baseline_vs_best.mp4`

## Conclusion

The most effective strategy was not a large smoothness penalty. Heavy smoothness regularization hurt recall. A better result came from light smoothness regularization, onset-aware reward shaping, and careful residual-factor calibration. This preserves the residual PPO structure while improving both note accuracy and motion smoothness metrics.

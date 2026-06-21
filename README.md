# Dexterous Piano Course Project Results

This repository contains the report and selected artifacts for our Dexterous Piano course project based on PianoMime. The project covers baseline reproduction, single-song specialist improvement, and multi-song generalist improvement.

## Top-level Files

```text
DRL_HW/
  Final_Report.pdf
  README.md

  baseline/
  Milestone_improvement/
  Final_improvement/
```

- `Final_Report.pdf`: final report.
- `baseline/`: reproduced PianoMime baseline results.
- `Milestone_improvement/`: full intermediate improvement records, metrics, method notes, and diagnostic summaries.
- `Final_improvement/`: final reader-facing videos.

## Baseline Results

`baseline/` stores the reproduced PianoMime behavior.

Single-task specialist baseline clips:

```text
baseline/single_task/Happy_8/final_video.mp4
baseline/single_task/ImagineDragons_6/imaginedragons.mp4
baseline/single_task/LetMeDownSlowly_6/letmeslowdown.mp4
```

Reproduced single-task F1:

| Clip | F1 |
|---|---:|
| `Happy_8` | `0.8725` |
| `ImagineDragons_6` | `0.9158` |
| `LetMeDownSlowly_6` | `0.9289` |

Multi-task/generalist baseline clips:

```text
Alone_1
EyesClosed_1
Hope_1
SomewhereOnlyWeKnow_1
NoTimeToDie_1
```

The five-clip generalist baseline mean F1 is `0.6150`.

Precision, recall, and F1 values are reported from the evaluator summaries. Mean F1 is not recomputed from the displayed mean precision and mean recall.

## Milestone Improvement Results

`Milestone_improvement/` keeps the detailed experiment records.

Important files:

```text
Milestone_improvement/single_task/summary.md
Milestone_improvement/single_task/summary.csv
Milestone_improvement/multi_task/summary.md
Milestone_improvement/multi_task/summary.csv
```

The final single-task specialist improvement is:

```text
Method: onset-aware reward + light smoothness regularization + residual factor calibration
Clip: Happy_8
F1: 0.8725 -> 0.9192
Delta F1: +0.0467
```

The main positive multi-task/generalist results are:

| Method | Mean F1 | Baseline F1 | Delta F1 |
|---|---:|---:|---:|
| Motion-weighted low-level diffusion fine-tune, `alpha = 3` | `0.6258` | `0.6150` | `+0.0108` |
| Smooth diffusion objective | `0.6256` | `0.6150` | `+0.0105` |
| Goal side-channel representation | `0.6235` | `0.6150` | `+0.0085` |
| Reward-based rollout selection | `0.6196` | `0.6150` | `+0.0046` |

RP1M-related methods are kept as diagnostic results because their five-clip averages are below the baseline, even though some single-clip diagnostics are positive after representation alignment and teacher recovery.

## Final Improvement Videos

`Final_improvement/` is the compact final video folder.

Current structure:

```text
Final_improvement/
  With_arm.mp4

  single_task/
    Happy_8.mp4
    ImagineDragons_6.mp4
    LetMeDownSlowly_6.mp4

  multi_task/
    Alone_1_motion_a3.mp4
    EyesClosed_1_motion_a3.mp4
    Hope_1_motion_a3.mp4
    NoTimeToDie_1_motion_a3.mp4
    SomewhereOnlyWeKnow_1_motion_a3.mp4
```

`Final_improvement/single_task/Happy_8.mp4` is intended to show the final single-task specialist improvement:

```text
onset-aware reward + light smoothness regularization + residual factor calibration
```

`Final_improvement/single_task/ImagineDragons_6.mp4` and `Final_improvement/single_task/LetMeDownSlowly_6.mp4` are kept as qualitative single-task reproduction/reference videos. The reported final single-task improvement is on `Happy_8`.

The `*_motion_a3.mp4` files show the final multi-task/generalist videos using:

```text
motion-weighted low-level diffusion fine-tune, alpha = 3
```

In the report, this is the strongest average generalist setting in the five-clip comparison:

```text
baseline mean F1 = 0.6150
motion_a3 mean F1 = 0.6258
delta F1 = +0.0108
```

Per-clip F1 for `motion_a3`:

| Clip | F1 |
|---|---:|
| `Alone_1` | `0.625` |
| `EyesClosed_1` | `0.478` |
| `Hope_1` | `0.667` |
| `SomewhereOnlyWeKnow_1` | `0.571` |
| `NoTimeToDie_1` | `0.789` |

`Final_improvement/With_arm.mp4` is the course bonus qualitative video for the arm-mounted Shadow Hand setup. It corresponds to the `Arm-Mounted Shadow Hands` section in the final report: two UR10-style arms carry the Shadow Hands, and the policy is produced through reference-based retargeting, action-sequence optimization, and BC/DAgger-style distillation. The reported no-clamp arm-mounted result is `Happy_8` F1 `0.8073` with precision `0.7309` and recall `0.9016`. This video should be interpreted as the bonus arm embodiment demo, not as one of the five `motion_a3` generalist benchmark videos.

# Dexterous Piano Course Project Results

This folder contains the experimental results for our Dexterous Piano course project. The experiments are based on PianoMime and cover both single-song policies and multi-song generalist policies.

The results are grouped into two parts:

- `baseline`: reproduction of the original PianoMime setting.
- `improvement`: evaluation of our optimization attempts against the corresponding baseline.

## Structure

```text
course_project_results/
  README.md

  baseline/
    single_task/
      song_name/
        training_curve_f1.png
        final_video.mp4

    multi_task/
      baseline_generalist/
        README.md
        metrics.csv
        figures/
        videos/
        run_config.md
      summary.csv

  improvement/
    single_task/
      01_method_name/
      02_method_name/
      combined_optimization/

    multi_task/
      01_method_name/
      02_method_name/
      combined_optimization/
      summary.md
      summary.csv
```

## Baseline Results

`baseline/single_task/` contains the original PianoMime single-song policy results for three training clips. Each clip folder keeps only the F1 training curve and the final performance video.

`baseline/multi_task/` contains the original PianoMime generalist policy results on unseen/test songs, including per-song metrics and selected videos.

## Selected Clips

All selected pieces are clips rather than full songs.

Single-task baseline clips from `dataset/notes/`:

```text
Happy_8
ImagineDragons_6
LetMeDownSlowly_6
```

`Happy_8` is relatively difficult and is also used as the fixed single-task comparison clip for later improvements.

Multi-task/generalist evaluation clips from `dataset/notes_test/`:

```text
Alone_1
EyesClosed_1
Hope_1
SomewhereOnlyWeKnow_1
NoTimeToDie_1
```

The same five test clips are used for the baseline generalist policy and all multi-task/generalist improvements.

## Improvement Results

The improvement section is organized by method rather than by song.

For `improvement/single_task/`, each method folder keeps only:

- `training_curve_f1.png`
- `final_video.mp4`

For `improvement/multi_task/`:

- `01_method_name/`, `02_method_name/`, etc. store individual optimization attempts.
- `combined_optimization/` stores the final combined method, if applicable.
- `summary.md` gives the human-readable conclusion.
- `summary.csv` gives the compact quantitative comparison.

Single-task baseline and improvement folders use the following compact format:

```text
training_curve_f1.png
final_video.mp4
```

For single-task methods, the curve and video are the required reader-facing artifacts.

For multi-task methods, all methods are evaluated on the same five unseen/test songs and compared against the baseline results stored under `baseline/multi_task/`.

## Table Format

Multi-task `metrics.csv` contains raw evaluation results:

```text
song_or_clip, split, seed, method, precision, recall, f1, notes
```

Multi-task `comparison_to_baseline.csv` contains baseline-vs-method results:

```text
song_or_clip,
baseline_f1,
improved_f1,
delta_f1,
notes
```

## File Types

The result folders mainly contain:

- F1 training curves and final performance videos for single-task policies.
- CSV tables for Precision, Recall, and F1 in multi-task/generalist experiments.
- Figures for multi-task method comparisons.
- MP4 videos for qualitative performance comparison.
- Short notes describing the multi-task method, evaluation setting, and selected checkpoint.

## Method README

Multi-task optimization folders may use a single `README.md` to describe the method. It should explain:

- What the method changes compared with the baseline.
- Why this change may improve the policy.
- Which setting it is evaluated on: single-task or multi-task/generalist.
- Which songs or clips are used.
- The main quantitative result.
- The final conclusion for this method.

`run_config.md` is only used when compact reproducibility details are needed, such as seed, checkpoint name, and evaluation command.

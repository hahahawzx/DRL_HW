# Run Config

- Method: goal side-channel representation
- Checkpoint: `low_level_goal_side_ft120.ckpt`
- Source experiment: `pianomime/results/improvements/course_project_2026/experiments/goal_representation/005_low_level_goal_side_channel`
- AE checkpoint: `checkpoint_ae.ckpt`
- Dataset: `dataset_ll_side_channel.zarr`
- Conditioning dimension: `436`
- Observation padding for online rollout: `32`
- Seed: `42`; per-clip seeds increment by clip order
- Diffusion inference iterations: `50`
- Evaluation clips: same five selected `dataset/notes_test/` clips used by the baseline generalist policy
- Video recording: enabled via `--record-dir`

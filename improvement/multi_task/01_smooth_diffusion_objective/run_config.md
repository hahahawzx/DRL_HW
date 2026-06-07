# Run Config

- Method: smooth diffusion objective
- Checkpoint: `low_level_smooth_deriv_seed42_e3_lr3e-5_t30_epoch003.ckpt`
- Base checkpoint: original `checkpoint_low_level.ckpt`
- AE checkpoint: `checkpoint_ae.ckpt`
- Dataset: `pianomime/dataset_ll.zarr`
- Seed: `42`; per-clip seeds increment by clip order
- Diffusion inference iterations: `50`
- Evaluation clips: same five selected `dataset/notes_test/` clips used by the baseline generalist policy
- Video recording: enabled via `--record-dir`

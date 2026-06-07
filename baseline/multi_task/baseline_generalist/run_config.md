# Run Config

- Setting: multi-task/generalist baseline
- Method: original PianoMime low-level diffusion policy
- Split: test
- Seed: `42`
- Checkpoint: `checkpoint_low_level.ckpt`
- Goal autoencoder checkpoint: `checkpoint_ae.ckpt`
- Dataset statistics: `pianomime/dataset_ll.zarr`
- Evaluation script: `pianomime/results/pandora_incremental_reproduction/eval_protocols/eval_clip_list.py`
- Diffusion steps: `50`
- IK mode: disabled

# Multi-task Generalist Baseline

This folder reports the original PianoMime generalist policy on five unseen test clips.

The policy uses the provided PianoMime checkpoints:

- Low-level policy: `checkpoint_low_level.ckpt`
- Goal autoencoder: `checkpoint_ae.ckpt`

The evaluation uses the same five clips listed in `clip_list.txt`. Results are summarized in `metrics.csv`, and the qualitative videos are stored under `videos/`.

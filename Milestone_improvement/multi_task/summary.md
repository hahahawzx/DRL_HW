# Multi-task / Generalist Improvement Summary

This section summarizes PianoMime multi-song / generalist-policy experiments. It does not include the single-song residual PPO experiment.

## Evaluation Setting

The multi-song policy follows PianoMime's two-stage diffusion generalist pipeline:

```text
MIDI goal / robot state
  -> high-level diffusion policy: predicts future fingertip trajectory
  -> low-level diffusion policy: predicts 46-D hand action from fingertip trajectory + proprioception
  -> RoboPianist / PianoMime environment: evaluates Precision, Recall, and F1
```

Default checkpoints:

```text
checkpoint_high_level.ckpt
checkpoint_low_level.ckpt
checkpoint_ae.ckpt
```

Default datasets:

```text
pianomime/dataset_hl.zarr
pianomime/dataset_ll.zarr
```

Default inference parameters:

| Module | pred_horizon | action_horizon | obs_horizon | obs_dim | action_dim | diffusion iters |
|---|---:|---:|---:|---:|---:|---:|
| high-level | 4 | 1 | 1 | 212 | 36 | 100 |
| low-level | 4 | 4 | 1 | 404 | 46 | 50 |

The reader-facing comparison in this folder uses the same five unseen clips for every method:

```text
Alone_1
EyesClosed_1
Hope_1
SomewhereOnlyWeKnow_1
NoTimeToDie_1
```

The baseline generalist mean F1 on these five clips is `0.615026`.

## Compared Methods

### Smooth Diffusion Objective

This is a low-level diffusion training modification. The motivation is that a diffusion action model may predict note-correct actions that are locally noisy or temporally inconsistent. We keep the original noise-prediction loss, reconstruct the denoised clean action estimate `a_hat_0` from the predicted noise, and add trajectory supervision on low-noise timesteps.

The implemented objective uses first-order delta matching, second-order jerk matching, a light absolute jerk penalty, and an `x0` anchor:

```text
L = L_noise
  + 1e-2 * ||Delta a_hat_0 - Delta a_demo||_2^2
  + 5e-3 * ||Delta^2 a_hat_0 - Delta^2 a_demo||_2^2
  + 1e-3 * ||Delta^2 a_hat_0||_2^2
  + 1e-3 * ||a_hat_0 - a_demo||_2^2
```

The extra terms are applied only for low-noise diffusion timesteps (`t <= 30`) with linear low-noise weighting. This is different from test-time low-pass smoothing, which was tested separately and reduced F1 by delaying or suppressing contact timing.

Result on the five-clip benchmark:

- mean F1: `0.625574`
- delta F1: `+0.010548`

This is the strongest average improvement in the current five-clip reader-facing comparison.

### Goal Side-channel Representation

This is a low-level conditioning modification. The motivation is that the same robot state can require different actions under different upcoming note patterns. We augment the low-level conditioning with an explicit goal side-channel, giving the policy clearer future note information.

Result on the five-clip benchmark:

- mean F1: `0.623506`
- delta F1: `+0.008480`

This is the second strongest five-clip result and is especially helpful on `EyesClosed_1`.

### Reward-based Rollout Selection

This is a test-time inference trick, not a training improvement. It keeps the official high-level and low-level checkpoints fixed, samples multiple stochastic diffusion rollouts, and selects the better rollout by reward / F1 proxy.

Related full 12-song result:

- official seed-0 baseline mean F1: `0.6095`
- reward-selected best-of-N mean F1: `0.6165`
- delta F1: `+0.0070`

Reader-facing five-clip result in this folder:

- mean F1: `0.619582`
- delta F1: `+0.004556`

This method should be reported as generative-policy sampling / best-of-N inference, not as a newly trained model.

### RP1M Low-level Replacement

This is a data-integration experiment. Direct RP1M usage does not work reliably unless the schema is aligned with PianoMime. The effective version uses point-major fingertip conversion and a learned linear mapping from RP1M action representation to PianoMime-style hand state.

Important single-clip result from the broader evaluation:

- `Forester_1` official baseline F1: `0.6731`
- RP1M point-major + learned linear-q, seed 3 F1: `0.6986`
- delta F1: `+0.0254`

Reader-facing five-clip result in this folder:

- mean F1: `0.595610`
- delta F1: `-0.019416`

This method is therefore useful evidence for the RP1M integration idea, but it is not stable on the selected five-clip benchmark.

### RP1M Recovery with Teacher Regularization

This is a trained low-level recovery method. It starts from the RP1M-derived low-level model, then recovers on original PianoMime low-level data while regularizing the student against the frozen official low-level teacher.

Training loss:

```text
loss = diffusion_noise_mse + lambda * mse(student_noise_pred, teacher_noise_pred)
```

Broader evaluation shows that teacher regularization matters:

- fixed-seed comparison improves `Forester_1` from `0.6731` to about `0.6897`
- removing the teacher drops F1 to `0.6518`
- recovery length around 50-100 batches works better than very short or very long recovery

Reader-facing five-clip result in this folder:

- mean F1: `0.609421`
- delta F1: `-0.005605`

This method is a meaningful training attempt, but it is not a stable improvement on the five selected clips.

## Negative / Diagnostic Findings

- Increasing low-level DDPM denoising steps from 50 to 75 did not help and degraded timing.
- Simple low-pass action smoothing reduced F1 because piano contact timing is sensitive.
- High-level RP1M fine-tuning did not exceed the official high-level baseline in the tested settings.
- RP1M helps only after representation alignment; direct conversion or manual q mapping is unreliable.

## Reporting Guidance

Use three claims in the final report:

1. Baseline reproduction: the official two-stage PianoMime generalist can be reproduced on unseen songs.
2. Generative inference: stochastic diffusion sampling enables best-of-N / reward-selected inference gains without training a new model.
3. RP1M integration: learned representation alignment makes RP1M useful on `Forester_1`, but broader five-clip generalization remains unstable.

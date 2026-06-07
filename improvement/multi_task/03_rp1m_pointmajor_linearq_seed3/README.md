# RP1M Low-level Integration

## What changed

This experiment replaces the official PianoMime low-level checkpoint with an RP1M-derived low-level diffusion policy.

The important part is representation alignment. Direct RP1M conversion is unreliable, so the retained variant uses:

- point-major fingertip demo conversion, aligning RP1M fingertip order with PianoMime-style low-level demo features;
- learned linear mapping from RP1M `action46` to PianoMime-style `q_hand54`, fitted from the original `dataset_ll.zarr`;
- fine-tuning from the official PianoMime low-level checkpoint.

## Motivation

PianoMime's official low-level model may not cover enough state-action variation for unseen songs. RP1M provides much broader piano-playing trajectories, so the goal is to use RP1M to improve low-level generalization. The key challenge is that RP1M and PianoMime do not use exactly the same low-level observation/action schema.

## Broader evaluation findings

Direct or weakly aligned RP1M conversion is not enough:

```text
RP1M-only manual-q conversion: poor recall under RP1M stats
mixed original + RP1M manual-q: unstable
point-major demo + manual q: close to baseline but not clearly better
```

The best RP1M variant uses point-major demo conversion plus learned linear-q mapping:

```text
Forester_1 official baseline F1 = 0.6731
RP1M point-major + linear-q, seed3 F1 = 0.6986
delta F1 = +0.0254
```

## Five-clip result in this folder

The reader-facing five-clip benchmark uses:

```text
Alone_1
EyesClosed_1
Hope_1
SomewhereOnlyWeKnow_1
NoTimeToDie_1
```

Result:

```text
baseline mean F1 = 0.615026
improved mean F1 = 0.595610
delta F1        = -0.019416
```

## Conclusion

RP1M integration is promising only after schema alignment. It gives the strongest observed Forester_1 single unseen result, but it does not generalize reliably across the selected five-clip benchmark. The report should avoid saying that RP1M directly improves the generalist policy; the accurate claim is that learned representation alignment makes RP1M useful in some settings.

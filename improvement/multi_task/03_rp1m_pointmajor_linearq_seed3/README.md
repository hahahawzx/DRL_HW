# RP1M Low-level Replacement

## What changed

This method replaces the original low-level checkpoint with the RP1M-derived low-level checkpoint using point-major fingertip conversion and learned linear action-to-q mapping.

## Why it may help

RP1M may provide broader state-space coverage for low-level control and improve generalization to unseen clips.

## Setting

- multi-task / generalist
- seed: `3`
- test clips:
  - `Alone_1`
  - `EyesClosed_1`
  - `Hope_1`
  - `SomewhereOnlyWeKnow_1`
  - `NoTimeToDie_1`

## Main quantitative result

This method is not effective on the selected five-song evaluation subset.

Mean F1 over the five clips:

- baseline: `0.615026`
- improved: `0.595610`
- delta: `-0.019416`

## Conclusion

The RP1M low-level model helps on `Hope_1`, but degrades the other four selected clips. It is therefore not a good final method for this five-song benchmark slice.

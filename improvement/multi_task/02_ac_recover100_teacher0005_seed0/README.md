# 02 ac_recover100_teacher0005_seed0

## What changed

This method uses the A+C style two-stage recovery: first initialize from the RP1M-based low-level model, then recover on original PianoMime data with teacher regularization.

## Why it may help

The method is intended to combine RP1M coverage with original-data recovery, while using the official low-level policy as a teacher to reduce drift.

## Setting

- multi-task / generalist
- seed: `0`
- test clips:
  - `Alone_1`
  - `EyesClosed_1`
  - `Hope_1`
  - `SomewhereOnlyWeKnow_1`
  - `NoTimeToDie_1`

## Main quantitative result

This method is not reliably effective on the five selected test clips.

Mean F1 over the five clips:

- baseline: `0.615026`
- improved: `0.609421`
- delta: `-0.005605`

## Conclusion

Although this method helps on some clips, it is not a stable improvement for the selected five-song evaluation subset.

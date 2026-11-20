# SilentVoice — sign recognition models and avatar motion

Models and motion data used by the Android app **SilentVoice: Sign & Speech** (Orbitray, `io.orbitray.silentvoice`).
They are published here because they are adapted from datasets licensed under a variant of
**CC BY-SA 4.0** (share-alike). The app's source code is not part of this repository.

| Pack | Files | Signs | Trained on |
|---|---|---|---|
| `rsl/` — Russian Sign Language | `model.onnx` (fp16 ensemble, 22.6 MB), `calib.json`, `vocab.json`, `anim.bin`, `anim_index.json` | 1000 signs + 33 fingerspelling letters; 604 recognized after the quality gate | Slovo, Bukva |
| `ase/` — American Sign Language | `model.onnx` (fp16 ensemble, 6.9 MB), `calib.json`, `vocab.json`, `anim.bin`, `anim_index.json` | 250 signs; 216 recognized after the quality gate | PopSign ASL v1.0; "no sign" class also uses Slovo `__none__` clips |

- `model.onnx` — input: per-frame hand and body landmarks (MediaPipe Hand & Pose Landmarker), encoded as described
  in the app's feature spec; output: class probabilities and an embedding.
- `calib.json` — per-sign acceptance thresholds and the list of signs that passed the quality gate.
- `vocab.json` — sign ids, glosses, categories, recognized flag, look-alike signs.
- `anim.bin` + `anim_index.json` — avatar motion: one landmark trajectory per sign (medoid of real performances).

No videos or images of people are included — only landmark-based models and landmark trajectories.

## Sources and attribution

- **Slovo: Russian Sign Language Dataset** — Alexander Kapitanov, Karina Kvanchiani, Alexander Nagaev,
  Elizaveta Petrova (SberDevices, ai-forever). License: variant of CC BY-SA 4.0.
  https://github.com/ai-forever/slovo
- **Bukva: Russian Sign Language Alphabet Dataset** — ai-forever (SberDevices). License: variant of CC BY-SA 4.0.
  https://github.com/ai-forever/bukva
- **PopSign ASL v1.0** — Thad Starner et al., Georgia Institute of Technology, Google (NeurIPS 2023 Datasets and
  Benchmarks). License: CC BY 4.0. https://signdata.cc.gatech.edu/view/datasets/popsign_v1_0/
- Landmarks extracted with **MediaPipe** (Google, Apache License 2.0).

## Changes made to the source data

Videos were not redistributed. Hand and body landmarks were extracted from the videos with MediaPipe Tasks;
neural networks were trained on these landmarks and exported to ONNX (fp16 weights). Avatar motion is a
selected real performance per sign (medoid by dynamic time warping), resampled and smoothed. Thresholds were
calibrated on held-out signers.

## License

The files in this repository are licensed under the **Creative Commons Attribution-ShareAlike 4.0
International** license (CC BY-SA 4.0), in line with the licenses of the source datasets — see `LICENSE`.
If you use them, keep the attribution above and share derived works under the same license.

Contact: orbitraystudio@gmail.com

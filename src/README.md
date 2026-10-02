# Source

Planned code for **DroneTrace** (Team 3). These modules are not written yet. No training pipeline has been run.

Room geometry and the microphone array are parameters. The code is not written around a single room or a fixed two-channel input.

| Planned file | Role |
| --- | --- |
| `simulate.py` | Acoustic simulation in Pyroomacoustics: randomized rooms, paths, absorption, gain, noise, and array layouts. The drone is blade-pass harmonics plus broadband noise. |
| `features.py` | Per-frame features: log-spectrograms, inter-channel phase difference (sine and cosine), level difference, and pairwise GCC-PHAT. |
| `baseline.py` | GCC-PHAT bearing plus a calibrated level-versus-distance curve, mapped to position. |
| `model.py` | CRNN with either direct position regression or a probability heatmap over candidate locations. |
| `train.py` | Training loop. Planned grid loss: cross-entropy on a Gaussian-smoothed target, plus a weighted position error. |
| `track.py` | Constant-velocity Kalman filter on per-frame heatmaps. Doppler radial velocity is a later input. |

Before the midterm the order is simulator and features, then the GCC-PHAT baseline, then the first CRNN trained on simulation and tested on real hover recordings.

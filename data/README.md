# Data

Audio for **DroneTrace** (Team 3) will live here. This folder currently holds only this note. Do not commit recordings, simulated waveforms, or model caches. See `.gitignore`.

## Planned sources

1. **Simulated.** Pyroomacoustics scenes with randomized room geometry, drone positions and paths, absorption, gain, noise, and array layout. Not generated yet.
2. **UaVirBASE.** Public multichannel UAV recordings with distance, altitude, azimuth, and orientation labels. Download it separately and do not vendor the dataset in git. We may use the full array or a subset of channels.
3. **Our recordings.** Hover trials and simple flight paths. Hover labels come from surveyed markers. Trajectory labels come from an overhead camera. Channel count is part of the recording setup, not fixed in advance. These will be stored locally or on course storage, not in this repo.

## Splits

Split by session and trajectory. Adjacent frames from the same flight must not land in both train and test.

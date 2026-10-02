# DroneTrace: Acoustic Drone Trajectory Reconstruction

Recover a drone’s trajectory from the sound it makes.

**11-785 Introduction to Deep Learning, Carnegie Mellon University.**

| | |
| --- | --- |
| **GitHub repo** | `dronetrace` |
| **Repo description** | DroneTrace: acoustic drone trajectory reconstruction. |
| **Status** | Proposal skeleton. No model, baseline, or result has been implemented. |

## Problem

A flying drone is loud, and that sound carries information about where it is and how it moves. The goal of this project is acoustic trajectory reconstruction: estimate the drone’s position over time from microphone recordings, then recover the path it flew.

GPS fails indoors, cameras can be blocked or raise privacy concerns, and motion-capture rigs are impractical. Sound still works in the dark and around corners. The hard part is that a short microphone array gives a strong bearing and a weak distance cue, and echoes, rotor-speed changes, and room differences all shift the signal.

We are not limiting the project to one room or to a two-microphone array. Room size, reverberation, and channel count are experimental factors. The first concrete setting is indoor flight, including a known room and a small array, because that is where we can simulate the physics and collect our own labels. Later experiments can change the room and the array.

We build toward a full trajectory in stages:

1. Locate a hovering drone.
2. Estimate its motion profile (per-frame position, distance, and speed).
3. Fuse those estimates into a smooth trajectory.

Initial targets are modest: hover error on the order of tens of centimeters, and a clear gain over the baseline. These numbers are goals, not measured results.

## Dataset

Public acoustic UAV sets rarely combine a drone source, a fixed external array, and time-synchronized trajectory labels. We plan to use three sources. Details and split rules are in [`data/README.md`](data/README.md).

- **Simulated.** Pyroomacoustics rooms with randomized geometry, drone positions and paths, absorption, gain, and noise. The drone is modeled as harmonics at a random blade-pass frequency plus broadband noise. Room and array layout are simulation parameters, not fixed to one setup.
- **Public real.** [UaVirBASE](https://github.com/Acoustic-UAV-Detection/UaVirBASE): synchronized multichannel UAV recordings with distance, altitude, azimuth, and orientation. Useful for pretraining and for testing how channel count affects localization.
- **Our recordings.** Hover trials and simple flight paths, with ground truth from surveyed markers for hover and an overhead camera for trajectories.

## Related work

- Knapp and Carter (1976), GCC-PHAT time-delay estimation. This is the baseline.
- Salvati, Drioli, and Foresti (2021), CNN-parametrized GCC-PHAT under reverberation.
- Vera-Diaz, Pizarro, and Macias-Guarasa (2018), a CNN that maps array audio to source position, pretrained on semi-synthetic data and fine-tuned on a little real data.

How each paper connects to this project is in [`related_work.md`](related_work.md).

## Baseline

GCC-PHAT bearing, plus a calibrated level-versus-distance curve, mapped to position. This is the planned baseline. It has not been implemented.

## Proposed approach

Per STFT frame we will compute per-channel log-spectrograms, inter-channel phase difference (sine and cosine), level difference per band, and GCC-PHAT features across microphone pairs. A CRNN maps these features either to a direct position regression or to a probability heatmap over candidate locations. The heatmap can represent ambiguities caused by reflections. For a moving drone, per-frame heatmaps would feed a constant-velocity Kalman filter whose measurement covariance comes from the heatmap spread. Doppler radial velocity from the rotor harmonics is a later extension.

| Model | Inputs to outputs | Stage |
| --- | --- | --- |
| Baseline | GCC-PHAT and band energy to position | Hover |
| CRNN-Reg | Spectrograms, IPD, ILD, and GCC-PHAT to position | Hover |
| CRNN-Grid | Same features to a heatmap over candidate locations | Hover |
| CRNN-Grid + tracker | Heatmaps, later plus Doppler, to position and velocity | Motion |

The planned code layout is in [`src/README.md`](src/README.md). None of these files exist yet.

## Evaluation

Hover: RMSE in meters,

\[
\mathrm{RMSE} = \sqrt{\frac{1}{N}\sum_i \|\hat{\mathbf{x}}_i - \mathbf{x}_i\|_2^2},
\]

plus median error and bearing error. Motion: speed mean absolute error and trajectory RMSE, also in meters.

Planned loss for the grid model, with \(p^\star\) a Gaussian-smoothed one-hot target over the grid and \(\mathbf{c}_g\) the cell centers:

\[
\mathcal{L} = \mathrm{CE}(\hat{p}, p^\star) + \lambda \left\| \sum_g \hat{p}_g \mathbf{c}_g - \mathbf{x} \right\|_2^2.
\]

## Planned experiments

1. Baseline: GCC-PHAT plus a calibrated level-to-distance curve.
2. Simulation-only training versus simulation plus fine-tuning on real hover data.
3. Heatmap output versus direct regression.
4. Array and room variation, including channel count, microphone spacing, and more than one room.

What we expect, and have not measured: the baseline should give a reasonable bearing and poor range; the CRNN should reduce range error; a simulation-only model should degrade on real audio until it is fine-tuned. There are no validation results yet. Experiment notes live in [`experiments/README.md`](experiments/README.md).

## Plan before the midterm

- Build the acoustic simulator and the feature pipeline, with room and array layout as parameters.
- Record an initial hover set.
- Implement and report the GCC-PHAT baseline.
- Train the first CRNN on simulation and test it on real hover recordings.

Later extensions are Doppler radial velocity, Kalman smoothing, and tests across rooms and array sizes.

## Repository layout

```text
├── README.md
├── related_work.md
├── proposal_draft.tex
├── requirements.txt
├── src/README.md
├── experiments/README.md
└── data/README.md
```

`requirements.txt` lists libraries we expect to use later. They are not pinned, and this repository does not import them yet.

## Authors

Seetharam Killivalavan, Rohith Arumugam Suresh and Keya Kesani
**Mentors:** Sarthak Bisht and Yixiong Fang


Kenneth C. Griffin School of Computer Science, Carnegie Mellon University

<!-- ## Acknowledgments

Carnegie Mellon University, the Language Technologies Institute, Bradley Warren, and Professor Bhiksha Raj for research guidance and support. -->

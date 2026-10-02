# Experiments

Planned comparisons for **DroneTrace** (Team 3). No runs, checkpoints, plots, or numbers belong here until a model has actually been trained and evaluated.

Room and array are factors in the experiments, not fixed to one room or two microphones.

## Planned comparisons

1. **Baseline.** GCC-PHAT bearing plus a calibrated level-versus-distance curve. We expect a usable bearing and weak range. This has not been run.
2. **Simulation versus fine-tuning.** Train on simulated audio only, then fine-tune the same model on real hover data. A simulation-only model is expected to degrade on real audio until it is fine-tuned.
3. **Output head.** A heatmap over candidate locations versus direct position regression.
4. **Array and room.** Channel count, microphone spacing, and more than one room.

## Metrics

Computed only after a real run:

- Hover: RMSE in meters, median error, and bearing error.
- Motion: speed mean absolute error and trajectory RMSE in meters.

Do not commit placeholder metrics, synthetic plots, or a claim that the baseline has been reproduced.

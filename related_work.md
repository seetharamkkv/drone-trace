# Related work

Starting bibliography for **DroneTrace** (11-785, Team 3). We have not reimplemented any of these papers.

## Knapp and Carter (1976) — baseline

C. Knapp and G. Carter. The generalized correlation method for estimation of time delay. *IEEE Trans. Acoust., Speech, Signal Process.*, 24(4):320–327, 1976.

GCC-PHAT estimates the time difference of arrival between a microphone pair and tolerates moderate reverberation. It yields a bearing and says little about range. Our baseline is GCC-PHAT bearing plus a calibrated level-versus-distance curve, mapped to position. The project is not limited to a two-microphone array; pairwise GCC-PHAT still applies when more channels are available.

## Salvati, Drioli, and Foresti (2021)

D. Salvati, C. Drioli, and G. L. Foresti. Time delay estimation for speaker localization using CNN-based parametrized GCC-PHAT features. In *Interspeech*, 2021.

They learn a parametrized GCC-PHAT with a CNN, which improves delay estimates under reverberation. We plan to use a GCC-PHAT vector as one input channel alongside spectrograms, inter-channel phase difference, and inter-channel level difference, following this feature idea rather than copying their architecture.

## Vera-Diaz, Pizarro, and Macias-Guarasa (2018)

J. M. Vera-Diaz, D. Pizarro, and J. Macias-Guarasa. Towards end-to-end acoustic localization using deep learning: from audio signals to source position coordinates. *Sensors*, 18(10):3418, 2018.

A CNN maps raw microphone-array audio directly to 3D source position in a real room, pretrained on semi-synthetic data and fine-tuned on a small amount of real data. We adopt that training strategy with a drone as the source, and we add a tracking stage so position estimates become a trajectory. Channel count is a variable, not fixed at two.

## Gap

Public acoustic UAV datasets rarely combine a drone source, a fixed external array, and time-synchronized trajectory labels. This project studies acoustic trajectory reconstruction across rooms and array sizes. The first experiments start from indoor flight, including a known room and a small array, because that is where simulation and our own labels are easiest to control.

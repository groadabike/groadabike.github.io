---
layout: page
title: CAD1
description: Machine Learning Challenges to improve music for people with hearing loss
img: assets/img/logos/cadenza_logo.png
importance: 1
category: Cadenza Project
related_publications: true
giscus_comments: true
---

## Improving music listening for those with hearing impairment

Cadenza is a 4.5-year project that aims to improve music listening for people with hearing impairments.
It addresses the need for better music processing through machine learning challenges.

In the duration of this project, we ran five challenges covering issues in music audio quality and lyric intelligibility.

---

## [CAD1 - The first Cadenza Challenge](https://cadenzachallenge.org/docs/cadenza1/cc1_intro)

CAD1 {% cite roadabike2025_ojsp %} was the first challenge presented in March 2023.  
It included two listening scenarios: (A) Music over headphones, and (B) Music over car's loudspeakers.

(A) The scenario considered a listener listening to music over headphones without wearing their hearing aid, so the headphones had to compensate for the hearing loss.
The challenge was presented as a demixing/remixing task in which participants had to demix stereo tracks into vocal, drums, bass and other (``VDBO'') stems,
to allow these to be remixed in a way that improves audio quality. Unlike traditional demixing challenges,
this task used HAAQI to evaluate the VDBO stems and the remix. Additionally, the remixed signal were evaluated by a listening panel
using audio quality scales developed withing the project.

(B) This scenario was about listening to music over the car's loudspeakers in the presence of car noise.
Here, listeners are wearing their hearing aids. Participants' algorithms had to process the music played by the car stereo in a way that increases its audio quality, while accounting for the car noise.
For this, participants have access to: (i) the clean reference song, (ii) the audiogram of the listener,
and (iii) the car speed, which gives an estimate of the noise's power spectrum — participants did not have
access to the noise signal itself (this was not a noise-cancellation task).
The output signals were evaluated using both the objective metric HAAQI and a listening panel.

## Baseline systems

**Task 1 (headphones).** Two out-of-the-box source separation models were provided as baselines, both trained exclusively on MUSDB18-HQ with no extra augmentation data:
- **Baseline 1 – Hybrid Demucs**, a time-domain/spectrogram hybrid model available in TorchAudio.
- **Baseline 2 – Open-Unmix**, a purely spectrogram-based model available via `torch.hub`.

Each system demixes the stereo track into eight VDBO stems (left/right for vocals, drums, bass, other), applies NAL-R amplification and compression personalised to each listener's audiogram, and produces a remix by linearly summing the processed stems.

**Task 2 (car).** A single baseline was provided, applying a level constraint to the music mixture at the hearing aid microphones to prevent clipping caused by the NAL-R amplification.

### Results

Of the two Task 1 baselines, Demucs scored highest and none of the participant entries managed to beat it on HAAQI:

| Team     | HAAQI Avg | HAAQI L | HAAQI R | BAQ (listening panel) |
|----------|-----------|---------|---------|------------------------|
| **Baseline (Demucs)** | **0.706** | 0.703 | 0.709 | 41.68 |
| E012 | 0.684 | 0.681 | 0.688 | 41.47 |
| E005 | 0.677 | 0.675 | 0.679 | 42.16 |
| Baseline (Open-Unmix) | 0.638 | 0.635 | 0.642 | — |
| E014 | 0.530 | 0.538 | 0.523 | 33.16 |
| E015 | 0.475 | 0.480 | 0.470 | — |
| E021 | 0.440 | 0.432 | 0.447 | 42.75 |
| E017 | 0.275 | 0.275 | 0.276 | 41.80 |
| E016 | 0.270 | 0.311 | 0.229 | 38.66 |
| E022 | 0.217 | 0.279 | 0.156 | 35.65 |

*(HAAQI: Hearing Aid Audio Quality Index, 0–1 scale. BAQ: Basic Audio Quality, listener panel score.)*

No entrant surpassed the Demucs baseline — likely because it was already a strong, state-of-the-art demixing model, leaving little room for improvement. This finding directly motivated the design of the subsequent ICASSP 2024 Cadenza Challenge, which introduced loudspeaker reproduction and independent VDBO gains to make the task harder to "solve" with off-the-shelf separation alone.

All submitted signals and HAAQI scores are openly available on [Zenodo](https://zenodo.org/records/13271525).

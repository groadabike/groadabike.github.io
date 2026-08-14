---
layout: page
title: ICASSP 2024
description: Personalised remixing of music for hearing aid users, accounting for loudspeaker cross-talk
img: assets/img/logos/cadenza_logo.png
importance: 2
category: Cadenza Project
related_publications: true
giscus_comments: true
---

## [ICASSP 2024 Grand Challenge](https://cadenzachallenge.org/docs/icassp_2024/intro)

The ICASSP 2024 Cadenza Grand Challenge {% cite roadabike2025_ojsp %} ran alongside CAD1 and concluded in January 2024. It pushed the demixing/remixing problem introduced in CAD1 into a harder, more realistic setting: rebalancing music for a listener wearing hearing aids while sat in front of a pair of stereo loudspeakers, rather than over headphones.

### The scenario

Unlike CAD1's headphone task, the signals here are picked up by microphones at each ear, capturing sound from a pair of stereo loudspeakers in a dead room. Because of cross-talk between the loudspeakers — strongest at low frequencies, where wavelengths are largest — the spatial distribution of each instrument differs from the original left/right mix. This cross-talk was modelled using head-related transfer functions (HRTFs), meaning standard stereo demixing algorithms had to be adapted to cope with a frequency-dependent distortion not present in prior demixing challenges.

Participants had to estimate the VDBO (vocals, drums, bass, other) components from this cross-talk-affected mixture, remix them according to listener-specified gains for each stem, and apply a standard hearing-aid amplification (personalised via the listener's audiogram) to the result. Both causal/low-latency and non-causal approaches were accepted, though an explicit demixing stage wasn't required — end-to-end systems were also welcome.

Systems were evaluated using HAAQI on the final remixed stereo signal.

### Baseline systems

As with CAD1, two out-of-the-box source separation models were provided as baselines: **Hybrid Demucs** and **Open-Unmix**, both distributed via PyTorch. Each estimates the VDBO stems from the loudspeaker mixture, applies the target gains, remixes to stereo, and finishes with NAL-R amplification personalised to the listener.

### Results

| Rank | Team | ID | HAAQI |
|------|------|-----|-------|
| 1 | T022 | E047 | 0.6317 |
| 2 | T022 | E022 | 0.6309 |
| 3 | T003 | E003_sup | 0.5929 |
| 4 | T003 | E003 | 0.5923 |
| 5 | T011 | E011_aug | 0.5857 |
| 6 | T018 | E018 | 0.5849 |
| 7 | T011 | E011 | 0.5798 |
| 8 | T012 | E012 | 0.5731 |
| 9 | T046 | E046 | 0.5704 |
| **10** | **Baseline (HDemucs)** | — | **0.5697** |
| ... | | | |
| 16 | Baseline (Open-Unmix) | — | 0.5113 |
| 19 | T016 | E016 | 0.1436 |

Unlike CAD1, several entrants (T022, T003, T011, T018, T012, T046) **outperformed the Hybrid Demucs baseline** — a result the organisers attribute to the added complexity of loudspeaker cross-talk and independent per-stem gains, which left more room for improvement over an off-the-shelf demixing model than CAD1's simpler headphone task did.

All submitted signals and HAAQI scores are openly available on [Zenodo](https://zenodo.org/records/13285031).
---
layout: page
title: CAD1
description: A tutorial-style guide to the first Cadenza Challenge, its tasks, resources, and how to build on it
img: assets/img/cad1/cad1-thumbnail.png
importance: 1
category: Cadenza Project
related_publications: true
giscus_comments: true
---

<h2 style="display: flex; align-items: center; gap: 0.75rem; flex-wrap: wrap;">
  <img
    src="{{ 'assets/img/logos/cadenza_logo.png' | relative_url }}"
    alt="Cadenza project logo"
    style="height: 60px; width: auto;"
  >
  <span>First Cadenza Challenge (CAD1)</span>
</h2>

<div style="margin: 0 0 1.5rem 0;" markdown="1">

{% include figure.liquid path="assets/img/projects/cad1/cad1-banner.png" title="Abstract CAD1 banner showing a network-like flow of audio signals splitting and recombining to represent source separation, personalised processing, and remixing" class="img-fluid rounded z-depth-1" %}

</div>

This page is written as a practical introduction to the first Cadenza Challenge: what we built, what we learned, and how others can still use the materials.
Although the original challenge closed in July 2023, its data, submitted signals, and evaluation results remain openly available for further research.

If you are new to the project, the best way to start is reading the official
[CAD1 introduction](https://cadenzachallenge.org/docs/cadenza1/cc1_intro), which explains all the deatils of the challenge.
This page complements it with a more research-project-oriented summary.

---

## What we did

CAD1 {% cite roadabike2025_ojsp %} was the first Cadenza challenge, launched in March 2023 to study how machine learning could improve music listening for people with hearing loss.
The project combined:

1. **A realistic hearing-focused audio task** rather than a standard music information retrieval benchmark.
2. **Open baselines and shared evaluation** so participants could compare systems fairly.
3. **Objective and perceptual testing** using both HAAQI and listening-panel ratings.

The challenge covered two listening scenarios:

### Task A: Headphone listening without hearing aids

Listeners heard music over headphones and were not wearing hearing aids, so the system had to compensate directly for the hearing loss.
Participants processed stereo songs by separating them into vocals, drums, bass, and other (VDBO) stems, then remixing and amplifying them for a listener-specific audiogram.

### Task B: In-car listening with hearing aids

Listeners heard music over a car audio system while wearing hearing aids and in the presence of road noise.
Participants were given the clean song, the listener audiogram, and the car speed as a proxy for the noise spectrum.
The goal was not noise cancellation, but hearing-aware music processing that improved perceived audio quality in context.

Across both tasks, CAD1 framed music enhancement as a personalised remixing problem rather than a generic denoising or separation benchmark.

<div style="max-width: 650px; margin: 0 auto;"  markdown="1">

{% include figure.liquid path="assets/img/projects/cad1/task2-scenario.png" title="Diagram of the CAD1 Task 2 scenario, where a hearing-aid user listens to music through a car audio system in the presence of speed-dependent car noise" class="iimg-fluid rounded z-depth-1" %}

***Figure 1**. The baseline for the headphone listening scenario. For simplicity, not all signal paths are shown.*
</div>


## How the challenge worked

Participants submitted processed audio signals, which we evaluated in two complementary ways:

1. **HAAQI**, to estimate hearing-aid-related audio quality objectively.
2. **Listening tests**, to measure perceived quality with human listeners.

This mattered because a system could look good from a signal-processing perspective but still fail to improve the actual listening experience.

## Baseline systems

**Task 1 (headphones).** Two out-of-the-box source separation models were provided as baselines, both trained exclusively on MUSDB18-HQ with no extra augmentation data:
- **Baseline 1 – Hybrid Demucs**, a time-domain/spectrogram hybrid model available in TorchAudio.
- **Baseline 2 – Open-Unmix**, a purely spectrogram-based model available via `torch.hub`.

Each system demixes the stereo track into eight VDBO stems (left/right for vocals, drums, bass, other), applies NAL-R amplification and compression personalised to each listener's audiogram, and produces a remix by linearly summing the processed stems.

<div style="max-width: 650px; margin: 0 auto;"  markdown="1">

{% include figure.liquid path="assets/img/projects/cad1/task1-baseline.png" title="Block diagram of the CAD1 headphone baseline: a stereo music mixture is separated into vocals, drums, bass, and other stems, each stem is processed with listener-specific hearing-loss compensation, and the processed stems are remixed into a personalised stereo output" class="img-fluid rounded z-depth-1" %}
***Figure 2**. The baseline for the headphone listening scenario. For simplicity, not all signal paths are shown.*

</div>

**Task 2 (car).** A single baseline was provided, applying a level constraint to the music mixture at the hearing aid microphones to prevent clipping caused by the NAL-R amplification.

<div style="max-width: 650px; margin: 0 auto;"  markdown="1">

{% include figure.liquid path="assets/img/projects/cad1/task2-baseline.png" title="Block diagram of the CAD1 car-listening baseline: the clean music signal is adjusted using listener audiogram information and car-speed-based noise estimates, with level control to avoid clipping at the hearing-aid microphones, producing a personalised in-car listening" class="img-fluid rounded z-depth-1" %}
***Figure 3**. The baseline for the car listening scenario. For simplicity, not all signal paths are shown.*

</div>

These baselines were important because they gave participants a concrete starting point and made it possible to understand whether a proposed method offered a real benefit over strong off-the-shelf systems.

## What we learned

For the headphone task, the Demucs baseline was already very strong and none of the submitted systems surpassed it on HAAQI.
That result was scientifically useful: it showed that Demucs source separation models are robust enough for studio recordings, leaving limited room for improvement when combined with hearing-aid-style amplification.

This directly informed the design of the later ICASSP 2024 Cadenza Challenge, where loudspeaker reproduction and cross-talk made the problem harder and created more headroom for participant methods.

### Selected results

Of the two Task 1 baselines, Demucs scored highest and none of the participant entries managed to beat it on HAAQI:

| Team                   | HAAQI Avg   | HAAQI L | HAAQI R | BAQ (listening panel) |
|------------------------|-------------|---------|---------|-----------------------|
| **Baseline (Demucs)**  | **0.706**   | 0.703   | 0.709   | 41.68                 |
| E012                   | 0.684       | 0.681   | 0.688   | 41.47                 |
| E005                   | 0.677       | 0.675   | 0.679   | 42.16                 |
| Baseline (Open-Unmix)  | 0.638       | 0.635   | 0.642   | —                     |
| E014                   | 0.530       | 0.538   | 0.523   | 33.16                 |
| E015                   | 0.475       | 0.480   | 0.470   | —                     |
| E021                   | 0.440       | 0.432   | 0.447   | 42.75                 |
| E017                   | 0.275       | 0.275   | 0.276   | 41.80                 |
| E016                   | 0.270       | 0.311   | 0.229   | 38.66                 |
| E022                   | 0.217       | 0.279   | 0.156   | 35.65                 |

*(**HAAQI**: Hearing Aid Audio Quality Index, 0–1 scale. **BAQ**: Basic Audio Quality, listener panel score, 0-100 scale.)*

## How to participate now

Even though the original competition is over, CAD1 is still useful as research resource.

You can participate by:

1. **Reading the original challenge docs** at [cadenzachallenge.org](https://cadenzachallenge.org/docs/cadenza1/cc1_intro) to understand the task definition.
2. **Downloading the public datasets** for [Task 1](https://zenodo.org/records/13285384) or [Task 2](https://zenodo.org/records/13329972)
3. **Reproducing the baselines** and comparing new systems against the published HAAQI results.
4. **Citing the challenge papers** if you build on the data or task.



## Open resources

- [Official CAD1 introduction](https://cadenzachallenge.org/docs/cadenza1/cc1_intro)
- [Submitted signals and HAAQI scores on Zenodo](https://zenodo.org/records/15738909)
- Our journal paper: {% cite roadabike2025_ojsp %}
- Listener panel paper: <br />Bannister, S., Firth, J., Roa-Dabike, G., Vos, R., Whitmer, W., Greasley, A. E., Graetzer, S., Fazenda, B., Cox, T., Barker, J., & Akeroyd, M. A. (2026). *The First Cadenza Challenge: Perceptual Evaluation of Machine Learning Systems to Improve Audio Quality of Popular Music for Those with Hearing Loss*. Trends in Hearing.

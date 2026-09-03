---
layout: page
title: CLIP1
description: A tutorial-style guide to the ICASSP 2026 Cadenza Lyric Intelligibility Prediction Challenge, its dataset, and how to build on it
img: assets/img/projects/clip1/clip1-thumbnail.png
importance: 3
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
  <span>ICASSP 2026 Cadenza Challenge: Predicting Lyric Intelligibility (CLIP1)</span>
</h2>

<div style="margin: 0 0 1.5rem 0;" markdown="1">

{% include figure.liquid path="assets/img/projects/clip1/clip1-banner.png" title="Abstract CLIP1 banner" class="img-fluid rounded z-depth-1" %}

</div>

This page is written as a practical introduction to the first Cadenza Lyric Intelligibility Prediction Challenge (CLIP1): what we built, what we learned, and how others can still use the materials.
Although the original challenge concluded at ICASSP 2026 in Barcelona, its dataset, submitted systems, and evaluation results remain openly available for further research.

If you are new to the project, the best way to start is reading the official
[CLIP1 introduction](https://cadenzachallenge.org/docs/clip1/intro), which explains all the details of the challenge.
This page complements it with a more research-project-oriented summary.

---

## Why this challenge?

In the Cadenza project, we want to improve music for people with hearing loss. Understanding the lyrics is a crucial part of music enjoyment.
However, unlike in spoken speech where we have STOI and HASPI, we don't have a well established metric on lyric intelligibility. 
Therefore, CLIP1 asked: can we automatically *predict* how intelligible the lyrics of a song are to a listener?

Understanding lyric intelligibility matters because:

- People with hearing loss often struggle to follow lyrics even when they can hear the music.
- In speech technology, intelligibility metrics have driven major improvements in speech enhancement. The same potential exists for music.
- No reliable metric existed for sung language, and existing speech metrics fail because sung and spoken language differ in rhythm, intonation, and production techniques.

CLIP1 was designed to catalyse this area by providing the first large-scale dataset of song excerpts paired with human intelligibility scores, and by challenging the community to build better prediction models.

---

## What we did?

Participants were given thousands of audio segments of accompanied singing from a diverse range of genres and styles. 
Their task was to predict the **word correct rate**, computed as the proportion of words correctly transcribed by normal-hearing listeners in a perceptual test.

The dataset contained audios in three conditions:

- **No hearing loss simulation** — the original signal.
- **Mild hearing loss simulated** — the original signal processed to model a mild audiogram.
- **Moderate hearing loss simulated** — the original processed to model a moderate audiogram.

This design meant that systems had to generalise across diverse hearing characteristics without necessarily requiring knowledge of hearing loss modelling.

---

## The CLIP dataset

The CLIP dataset {% cite ROADABIKE2026112466 %} is one of the main contribution of this challenge. 
It consists of 3,700 audio excerpts drawn from the FMA dataset, each paired with:

1. The stereo audio as heard by listeners (with or without hearing loss simulation).
2. The unprocessed audio without hearing loss simulation.
3. The severity of hearing loss applied.
4. Ground-truth lyric transcriptions.
5. Listener transcriptions and the resulting intelligibility score.

[//]: # (### How it was built?)

[//]: # ()
[//]: # (Tracks were selected from FMA-full by filtering for English songs with vocals, spanning genres from rap to rock. One chorus or verse per song was extracted using an automatic structure segmentation model, yielding over 17,000 candidate segments.)

[//]: # ()
[//]: # (Ground-truth transcriptions were produced by native English-speaking PhD students at the Universities of Sheffield and Salford, using Label Studio. Each annotator transcribed 500 randomly assigned segments. Post-processing removed repeated phrases and retained only excerpts containing five to ten words, resulting in the final 3,700 excerpts.)

[//]: # ()
[//]: # (Intelligibility scores were collected via Prolific, recruiting young adult native English speakers with normal hearing. Each excerpt was presented twice, with the first presentation serving as a familiarisation pass. Listeners transcribed what they heard, and the intelligibility score was computed as the ratio of correctly transcribed words to ground-truth words, after text normalisation to handle homophones and contractions.)

[//]: # ()
[//]: # (<div style="max-width: 650px; margin: 0 auto;" markdown="1">)

[//]: # ()
[//]: # ({% include figure.liquid path="assets/img/projects/clip1/clip1-data-diagram.png" title="Schematic of the CLIP1 dataset construction process, showing track selection, transcription, and listening test stages" class="img-fluid rounded z-depth-1" %})

[//]: # ()
[//]: # (***Figure 1.** Schematic of data generation and processing. Black lines indicate metadata and blue lines indicate audio.*)

[//]: # ()
[//]: # (</div>)

[//]: # ()
[//]: # (---)

## Baseline system

We provided two baselines:

1. A Whisper-based automatic speech recognition (ASR) approach adapted for sung speech. It applied music source separation to isolate the vocal track, then used Whisper to generate a predicted transcription, which was compared against the ground-truth text to estimate intelligibility.
2. A STOI-based model. This model employed the traditional STOI model and used the separated vocals as reference signal.

These two baseline were intentionally straightforward to give participants a concrete and well-understood starting point.

---

## Results

The challenge attracted **29 submitted systems** from teams around the world, with the top teams invited to present at ICASSP 2026 in Barcelona. Systems were evaluated using RMSE between predicted and human intelligibility scores, and Pearson correlation.

### Selected results

| Rank | System     | Ref Text | HL Severity | Eval RMSE | Eval Corr |
|------|------------|----------|-------------|-----------|-----------|
| 1    | T045 \*    | Yes      | Yes         | 26.44     | 0.67      |
| 2    | T071a \*   | Yes      | No          | 26.52     | 0.67      |
| 3    | T013a \*   | Yes      | No          | 26.54     | 0.67      |
| 4    | T017       | No       | No          | 26.67     | 0.67      |
| 5    | T072a      | Yes      | Yes         | 26.68     | 0.66      |
| 6    | T072b      | No       | Yes         | 26.84     | 0.66      |
| 7    | T040 \*    | No       | No          | 27.07     | 0.65      |
| 8    | T088a \*   | Yes      | Yes         | 27.22     | 0.65      |
| 9    | T051       | Yes      | No          | 27.31     | 0.64      |
| 10   | T086       | No       | Yes         | 27.31     | 0.64      |
| ...  |            |          |             |           |           |
| 17   | BL Whisper | Yes      | No          | 29.08     | 0.58      |
| 27   | BL STOI    | No       | No          | 34.89     | 0.21      |

*(\* Invited to present at ICASSP 2026. **Eval RMSE**: lower is better. **Eval Corr**: higher is better.)*

### What we learned?

Several participant systems substantially outperformed both baselines. The Whisper-based baseline, despite being a strong ASR system, left considerable room for improvement — confirming that lyric intelligibility prediction is a genuinely hard problem that benefits from purpose-built approaches.

The STOI baseline, adapted from speech intelligibility, performed poorly, validating the core motivation for the challenge: speech metrics do not transfer reliably to music.

The top systems generally made use of ground-truth reference text and modelled hearing loss severity explicitly, 
suggesting these are important signals for accurate prediction. However,
this comes with a trade-off: systems that rely on reference text or hearing loss information
are harder to deploy in practice, as such inputs are rarely available in real-world applications.
A fully agnostic system - one that requires only the audio - would generalise better across
different contexts and use cases. This tension between prediction accuracy and practical
applicability remains an open and interesting research question for future challenges.

---

## How to use the materials now?

Even though the competition is over, CLIP1 remains a useful open research resource.

You can get started by:

1. **Reading the original challenge docs** at [cadenzachallenge.org](https://cadenzachallenge.org/docs/clip1/intro).
2. **Downloading the dataset** from [Zenodo](https://zenodo.org/records/17950664).
3. **Reproducing the baselines** from [PyClarity](https://github.com/claritychallenge/clarity/tree/main/recipes/cad_icassp_2026) GitHub and comparing new systems against the published results.
4. **Citing the dataset paper** {% cite ROADABIKE2026112466 %} if you build on the data or task.
5. **Citing the ICASSP 2026 overview paper** {% cite roa-Icassp2026 %} 

---

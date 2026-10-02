---
layout: page
title: CLIP1
description: 
img: assets/img/projects/clip1/clip1-thumbnail.png
importance: 3
category: Cadenza Project
related_publications: true
giscus_comments: true
---

<h1 style="display: flex; align-items: center; gap: 0.75rem; ">
  <img
    src="{{ 'assets/img/logos/cadenza_logo.png' | relative_url }}"
    alt="Cadenza project logo"
    style="height: 100px; width: auto;"
  >  
    <span>ICASSP 2026 Cadenza Challenge:<br />Predicting Lyric Intelligibility</span>
</h1>

<div style="margin: 0 0 1.5rem 0;" markdown="1">

{% include figure.liquid path="assets/img/projects/clip1/clip1-banner.png" title="Abstract CLIP1 banner" class="img-fluid rounded z-depth-1" %}

</div>

This is a summary of CLIP1, the first Cadenza Lyric Intelligibility Prediction Challenge. 
Although the original challenge concluded and results were presented at ICASSP 2026 in Barcelona, 
the dataset, submitted systems, and evaluation results remain openly available for further research.

Note that if you want to understand the challenge and explore your own solutions, 
the best way to start is by visiting the official [CLIP1 website](https://cadenzachallenge.org/docs/clip1/intro), 
where you will find the challenge description, the rules for a fair competition and access to the dataset. 

---

## Why this challenge?

In the Cadenza project, we want to improve music for people with hearing loss. Understanding the lyrics is a crucial part of music enjoyment.
However, unlike in spoken speech where we have several speech intelligibility metrics, such as STOI and HASPI, we don't have 
a well established metric for estimating the lyric intelligibility from a piece of music.
That gap is what CLIP1 set out to address: can we automatically predict how intelligible the lyrics of a music excerpt are to a listener?

Addressing this matters for several reasons. 
In speech technology, intelligibility metrics have driven major advances in speech enhancement, and the same potential exists for music. 
That potential is especially important for people with hearing loss, who often struggle to follow lyrics even when they can hear the music. 
But we can't simply borrow speech metrics: sung and spoken language differ in rhythm, intonation and vocal production, so tools designed for speech don't transfer well.

CLIP1 was designed to kick-start research in this area. 
It provided the first large-scale dataset of song excerpts paired with human intelligibility scores, and challenged the community to build models that could predict them.

---

## What is the task?

To support this, we constructed a dataset of thousands of excerpts of accompanied singing, covering a diverse range of genres and styles. 
Each excerpt was presented to normal-hearing listeners in a perceptual test, and their transcriptions were used to compute a word correct rate: 
the proportion of words transcribed correctly. Challenge entrants were asked to predict this score.

To reflect the range of listeners Cadenza aims to support, each excerpt was presented in three conditions:

* **No hearing loss** -- the original signal.
* **Mild hearing loss** -- the original signal processed to simulate a mild audiogram.
* **Moderate hearing loss** -- the original signal processed to simulate a moderate audiogram.

This meant that systems had to predict intelligibility across different hearing profiles, not just for typical listeners.

---

## The CLIP dataset

The CLIP dataset {% cite ROADABIKE2026112466 %} is one of the main contributions of this challenge. 
It consists of 11,000 audio excerpts drawn from the FMA dataset, each paired with:

1. The stereo audio as heard by listeners (in one of the three hearing conditions above).
2. The unprocessed audio, without hearing loss simulation.
3. The severity of hearing loss simulated.
4. Ground-truth lyric transcriptions.
5. Listener transcriptions and the resulting intelligibility score.

## Challenge baselines

We provided two baselines:

1. Whisper-based: an automatic speech recognition (ASR) approach adapted for singing. Music source separation was first used to isolate the vocals, then Whisper transcribed them. The predicted transcription was compared with the ground-truth lyrics to estimate the word correct rate.
2. STOI-based: the standard STOI metric, using the separated vocals as the reference signal and the processed mixture as the degraded signal.

Both baselines were intentionally simple, giving entrants a concrete, well-understood starting point.

---

## Results

The challenge attracted **29 systems** from teams around the world, with five teams invited to present at ICASSP 2026 in Barcelona. 
Systems were evaluated using RMSE and Pearson correlation between predicted and human intelligibility scores.

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

## What we learned

Several submitted systems substantially outperformed both baselines. 
The Whisper-based baseline, despite using a strong ASR system, left considerable room for improvement, 
confirming that lyric intelligibility prediction is a genuinely hard problem that benefits from purpose-built approaches.

The STOI baseline performed poorly, validating the core motivation for the challenge: speech metrics do not transfer reliably to music.

The top systems generally used the ground-truth lyrics and modelled hearing loss severity explicitly, 
suggesting these are important signals for accurate prediction. 
However, this comes with a trade-off. Systems that rely on reference text or hearing loss information are harder to deploy, 
as such inputs are rarely available in real-world applications. 
An audio-only system would be far more widely applicable (e.g. [system T040](https://ieeexplore.ieee.org/document/11464690)). 
Balancing prediction accuracy against practical deployability remains an open and interesting question for future challenges.

---

## Using the materials

Although the challenge has closed, CLIP1 remains a useful open research resource. To get started:

1. **Read the original challenge docs at** [cadenzachallenge.org](https://cadenzachallenge.org/docs/clip1/intro).
2. **Download the dataset** from [Zenodo](https://zenodo.org/records/17950664).
3. **Reproduce the baselines** using the [PyClarity recipe](https://github.com/claritychallenge/clarity/tree/main/recipes/cad_icassp_2026), and compare new systems against the published results.
4. **Cite the dataset paper** {% cite ROADABIKE2026112466 %} if you use the data.
5. **Cite the ICASSP 2026 overview paper** {% cite roa-Icassp2026 %} if you build on the task or compare against challenge results.

---

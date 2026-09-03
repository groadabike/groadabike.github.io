---
layout: page
title: CLIP1
description: Cadenza Lyric Intelligibility Prediction 
importance: 2
category: Intelligibility
related_publications: true
img: assets/img/projects/clip1/clip1-thumbnail.png
giscus_comments: true
---

# How the CLIP Dataset Was Created

<div style="margin: 0 0 1.5rem 0;" markdown="1">

{% include figure.liquid path="assets/img/projects/clip1/clip1-banner.png" title="Abstract CLIP1 banner" class="img-fluid rounded z-depth-1" %}

</div>

A summary of the dataset construction methodology for the Cadenza Lyric Intelligibility Prediction (CLIP) dataset, published on {% cite ROADABIKE2026112466 %}

## Overview

CLIP is a dataset of 11,072 popular Western music signals sung in English, paired with ground truth lyric transcriptions and human-rated intelligibility scores. 
It was built to support the development of machine learning algorithms that predict how intelligible song lyrics are to listeners, including listeners with hearing loss,
for the Cadenza ICASSP 2026 Signal Processing Grand Challenge.

The music was drawn from the [Free Music Archive (FMA)](https://github.com/mdeff/fma), a source of independent-artist recordings unlikely to be familiar to listeners, 
this ensured that intelligibility scores reflected genuine lyric perception rather than prior familiarity with a song. 
Two-thirds of the audio was further processed with a hearing loss simulator, so that the dataset captures a more diverse range of listening 
ability than is typical in music datasets.

The construction pipeline had two broad stages: a **Selection stage**, 
in which suitable tracks were automatically filtered from FMA, 
and a **Process stage**, in which ground truth lyrics and human intelligibility scores were collected.

---

## Selection Stage

### 1. Filtering FMA for suitable tracks

The starting point was FMA-Full, containing 106,574 complete tracks. We applied a cascade of automatic filters, designed to minimize human bias in the track selection:

- **License filtering**: 42,735 tracks were excluded because their license did not explicitly permit derivative works (required since audio tracks would be segmented into small excerpts and a hearing-loss simulation would later be applied to the audio).
- **Genre filtering**: Using FMA's top-genre and sub-genre labels, 31,211 further tracks were removed for being labelled Instrumental, Experimental, Classical, or International, categories unlikely to contain Western popular music sung in English.

This left 29,831 candidate tracks.

### 2. Filtering for vocal presence

Tracks without reliably detectable vocals were removed using two automated tools: the [HTDemucs](https://github.com/facebookresearch/demucs) music source separation model, to isolate estimated vocal stems, and the [Silero VAD](https://github.com/snakers4/silero-vad) voice activity detection model, to detect vocal segments. Tracks were excluded if Silero VAD found no voice activity or if the RMS amplitude of the estimated vocal stem fell below -40 dBFS. This removed 3,428 tracks, leaving 26,403.

### 3. Selecting a chorus or verse

To maximize song diversity (favoring many short excerpts over fewer long ones), each track was reduced to a single chorus or verse section, 
identified using the [All-In-One Music Structure Analyzer](https://github.com/mir-aidj/all-in-one). This model successfully located a chorus or verse in 17,137 tracks, forming the pool from which excerpts were drawn.

--- 

## Process Stage

### 4. Ground truth transcription

From the 17,137 candidate sections, 3,500 were randomly selected for transcription. 
Seven native English-speaking PhD students from the Universities of Salford and Sheffield each transcribed 500 non-overlapping sections using Label Studio. 
To assist transcription, annotators had access to three audio versions per excerpt: the original, an HTDemucs-isolated vocal track, 
and a high-pass filtered version (300 Hz cutoff). 
They transcribed 5–10-word lyric phrases, flagged obscene content, marked unintelligible words, and cross-checked ambiguous lyrics against sites such 
as Genius and Bandcamp.

Transcribed phrases were post-processed for consistency: repeated phrases and excessively repeated single words were removed, 
and phrase length was normalized to 5–10 words (short phrases were merged, long ones trimmed after manual review). 
This yielded a final set of **3,700 excerpts from 1,452 tracks**. 
A small number of excerpts (34) retained "?" markers for words that were unintelligible even to expert transcribers.

### 5. Hearing loss simulation

Excerpts were split by artist into training (80%), validation (10%), and evaluation (10%) sets. 
Each excerpt was then processed with a validated hearing loss simulator (based on a gammatone auditory-periphery model) 
to produce versions simulating **mild** and **moderate** hearing loss, using audiograms drawn from the Cadenza CAD1 challenge pool, 
applied symmetrically to both ears. 
Severe and profound loss were excluded, as those listeners typically rely on hearing devices. 
Loudness was normalized (75 dB SPL pre-processing, RMS-matched post-processing, peak-normalized) to keep listening test volumes comparable across conditions. 
This produced **11,100 music signals**: an original version plus mild and moderate hearing-loss versions of each excerpt.

### 6. Listening tests

Human intelligibility scores were collected via online listening tests on Prolific during the second half of July 2025. The 11,100 signals were shuffled into 111 groups of 100, each assigned to a different participant, with no listener hearing two versions of the same excerpt (to avoid inflated scores from repeated exposure). 111 adults aged 18–40, all native English speakers who self-reported normal hearing, took part (median session length 42 minutes; participants paid £7.00 plus fees). Each signal was played twice; listeners typed what they heard, entering "xxx" if they could not identify any words.

Data quality was actively monitored: 13 participants who submitted an unusually high number of "xxx" responses were excluded and replaced, reducing the total "xxx" rate. A subsequent manual quality-control pass reviewed each excerpt against its transcription and participant feedback, discarding 28 further responses due to boundary errors, failed audio loads, empty responses, or duplicate tracks. This produced the final dataset of **11,072 music signals**, with durations ranging from 1.1–22.9 seconds (mean 4.5s).

Participant demographics: 50.5% female, 45.5% male; mean age 31.0 (SD 5.7); self-reported ethnicity 64.9% White, 22.5% Black, 6.3% Asian, 6.3% Mixed.

### 7. Computing intelligibility scores

Word-level intelligibility (**correctness**) was computed as the ratio of matching words between each ground truth transcription (prompt) and the listener's response, after normalizing both texts (expanding numbers and contractions, correcting misspellings, lower-casing). To avoid penalizing homophone spelling differences, both prompt and response were converted to phonemic form using the BEEP pronunciation dictionary (supplemented by a Phonetisaurus grapheme-to-phoneme model for out-of-dictionary words), and aligned using a Levenshtein-distance dynamic programming alignment (via the `jiwer` Python module). Where multiple valid transcriptions or phonemic readings existed, the alternative giving the highest score was used. A parallel **phoneme-level correctness** score was also computed from the phonemic alignments.

---

## Resulting Dataset

The final CLIP dataset contains **11,072 audio excerpts** (16-bit FLAC, 44.1 kHz) with associated ground truth lyrics, listener transcriptions, word- and phoneme-level intelligibility scores, and metadata (including hearing-loss condition), split into training, validation, and evaluation sets disjoint by artist and audiogram. Statistical testing (Kolmogorov–Smirnov) confirmed similar score distributions across the three splits, while hearing-loss severity produced significantly different — and expected — intelligibility distributions, with moderate loss producing the lowest scores.

The dataset is openly available on [Zenodo](https://zenodo.org/records/17950664).

---

## Key Limitations

- Ground truth transcriptions were manually generated (since most FMA tracks lack published lyrics) and may retain some transcriber error.
- Each excerpt received only a single intelligibility rating from one participant, rather than an averaged score across multiple listeners, prioritising musical coverage over sample measurement precision. This introduces an inter-annotator noise floor into the scores.

--- 
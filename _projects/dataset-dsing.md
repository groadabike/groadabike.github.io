---
layout: page
title: DSing
description: DAMP Sing
importance: 1
category: ASR
related_publications: true
img: assets/img/projects/dsing/dsing_menu.png
giscus_comments: true
---

{% include figure.liquid path="assets/img/projects/dsing/dsing.png" title="A karaoke microphone on a stand in front of a TV screen displaying song lyrics in a cozy home living room" class="img-fluid rounded z-depth-1" %}

DSing {% cite roadabike2019_interspeech %} is an unaccompanied singing dataset created to fill a gap in automatic lyrics transcription datasets.
It is derived from the [Smule DAMP-MVP 300x30x2](https://zenodo.org/records/2747436) dataset,
a collection of thousands of solo-singing karaoke recordings.

## Construction Process

I built this dataset following these steps:

1. **Source data** — I obtained the [Sing! 300x30x2](https://zenodo.org/records/2747436) dataset from the DAMP datasets collection.
2. **Language filtering** — I filtered the recordings and kept only songs with English lyrics.
3. **Lyrics download** — Using each song's unique ID, I downloaded the corresponding lyrics from the [Smule](https://www.smule.com) website.
4. **Lyrics alignment** — I aligned the audio recordings with the downloaded lyrics.
5. **Audio segmentation** — I split the audio into complete lyric phrases.

## Download the Data

The DSing [repository](https://github.com/groadabike/Kaldi-Dsing-task) contains the following directories:

- `DSing-Kaldi-Recipe/`: Kaldi recipe for the DSing ASR task (the baseline system).
- `DSing-preconstructed/`: the DSing dataset segmentation.

## Getting started

* Get the audio dataset: Request permission and download the audio from [Smule DAMP-MVP 300x30x2](https://zenodo.org/records/2747436).
* Get the dataset segmentation: The train/dev/test splits used in our paper are in [`DSing-preconstructed/`](https://github.com/groadabike/Kaldi-Dsing-task/DSing-preconstructed).
* Run the baseline system: See `DSing-Kaldi-Recipe/` for instructions on training and evaluating the baseline Kaldi ASR system.

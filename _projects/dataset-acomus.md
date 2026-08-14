---
layout: page
title: Acomus
description: Acoustic Covers of Music
importance: 2
category: ASR
related_publications: false
img: assets/img/projects/acomus/acomus_menu.png
---

{% include figure.liquid path="assets/img/projects/acomus/acomus.png" title="Female singer performing with a classical guitar" class="img-fluid rounded z-depth-1" %}

The Acoustic Covers of Music (ACOMUS) dataset born to partially cover the lack of musical corpus for sung speech recognition.
This corpus is composed of several acoustic covers versions of popular songs from YouTube.

The motivation of this project was to create a suitable corpus for my MSc Dissertation project on sung speech recognition.
This dataset was designed and constructed for academic porpoises only, and all the songs and its lyrics are
property of the creator artist, and all the credits belong to them.

This is a small dataset that can be used for evaluation and benchmarking.

## Characteristics

The corpus is designed and constructed with the following specifications:

1. The songs are mainly interpreted by **Amateur Artists** and around 10% are known artists.
2. The dataset is separated in a balanced number of **Male** and **Female** artists.
3. The interpretations have one accompaniment instrument, and just some few cases have more than one.
4. The 80% of the database are accompanied by acoustic guitar and 20% by piano.

**Table 1** summarizes the total and annotated duration of the corpus by accompaniment instrument:

<div id="table-annotated-time"></div>

<div style="max-width: 500px; margin: 0 auto;"  markdown="1">

***Table 1**: Total vs. annotated audio duration in ACOMUS, by instrument (guitar and piano).*

| Instrument | Annotated | Total Time | Annotated Time |
|:-----------|:---------:|:----------:|:--------------:|
| Guitar     |    100    |  389 min.  |    233 min.    |
| Piano      |    20     |  77 min.   |    49 min.     |

</div>

**Table 2** shows how the singers and songs are distributed by sex, along with the split between guitar and piano accompaniment for each:

<div id="table-sex-destribution"></div>

<div style="max-width: 500px; margin: 0 auto;"  markdown="1">

***Table 2**: Distribution of ACOMUS singers and songs by sex, with instrument breakdown (guitar vs. piano).*

| Sex    | Unique Singers | Total Songs | Guitar | Piano |
|:-------|:--------------:|:-----------:|:------:|:-----:|
| Female |       86       |     115     |   91   |  24   |
| Male   |       89       |     125     |  109   |  16   |

</div>

## Download

Currently, only the code to generate the dataset is available on [GitHub](https://github.com/groadabike/ACOMUS), and it is in Python 2.7.
I am planning on updating the code and on releasing the audio excerpts to stop relaying on YouTube.


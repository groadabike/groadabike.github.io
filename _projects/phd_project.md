---
layout: page
title: PhD Project
description: Deep Learning Approaches for Automatic Sung Speech Recognition
img: assets/img/12.jpg
importance: 1
category: PhD
related_publications: true
---

## Deep Learning Approaches for Automatic Sung Speech Recognition: Adapting Spoken Technologies to Sung Speech

I did my PhD at the University of Sheffield during the years 2018-2022. 

My research focuses on automatic sung speech recognition and lyric transcription using deep learning and audio signal processing techniques. 
Through this project, I explore the challenges of understanding singing voices, including the effects of musical accompaniment, 
pitch variation, and reduced intelligibility compared to spoken speech. 
The project combines research on dataset creation, vocal source separation, acoustic modelling, and machine learning methods for recognizing lyrics in music.

## Abstract

Automatic sung speech recognition is a challenging problem that remains largely unsolved. Challenges are due to both the intrinsic poor intelligibility of sung speech and the difficulty of separating the vocals from the musical accompaniment. In recent years, deep neural network techniques have revolutionised spoken speech recognition systems through advances in both acoustic modelling and audio source separation.

This thesis evaluates whether these new techniques can be adapted to work for sung speech recognition. For this, it first presents an analysis of the differences between spoken and sung speech. Then motivated by this analysis, the thesis makes four major contributions.

First, the thesis addresses the lack of large, standardised sung speech datasets suitable for evaluating sung speech recognition. The opportunity for building a suitable dataset has recently arisen with the release of Smule’s DAMP-MVP dataset, a large unaccompanied karaoke performance dataset. However, constructing a well-balanced and easy-to-use evaluation dataset from this weakly-labelled and weakly-annotated data presents many challenges. This thesis presents solutions to these challenges.

Second, the thesis reconsiders the problem of sung speech acoustic modelling. New musically-motivated features are considered to capture the importance of the vocal source information. Features considered include pitch, voicing degree, voice quality, and beat-based features. It is shown that pitch and voicing degree features are useful for improving recognition performances.

Third, accompanied sung speech recognition poses a challenging source separation problem. This thesis investigates the use of modern time-domain source separation networks. Also, it investigates whether ‘speaker embedding’ ideas can be employed for music source separation by considering the use of `instrument’ embeddings.

Finally, a complete system that combines the deep neural network based source separation and speech recognition components are jointly evaluated, dealing with the mismatch between the distorted sung speech originated from the separation network and the `clean’ sung speech used for acoustic modelling.

## Thesis

You can download my thesis from [White Rose eTheses Online](https://etheses.whiterose.ac.uk/id/eprint/30716/).

I also created an [accompanied website](https://thesis.gerardoroadabike.com/) for the project where I shared some audio recordings.

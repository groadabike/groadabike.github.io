---
layout: post
title: Working with MedleyDB
subtitle: and its python tools
image: 
bigimg: 
tags: [MedleyDB, Acomus]
date: 2018-03-12
published: True
---

# MedleyDB, a multitrack dataset.

MedleyDB [1] is a great and completely free dataset for music research
created at NYU's Music and Audio Research Lab.
This collection consist in 122 different multitracks of 44.1 kHz wav files.
Each of the components of the tracks are divided in a independent channel.  
For a detailed description of the dataset, please refer directly to the
[project website](http://medleydb.weebly.com/).

In addition to the dataset, in the website of the project is it possible to find 
a great set of [python tools](https://github.com/marl/medleydb)
that allows to easily work with the entirely collection. 

In this post we will focus in how this tools can be used in order to 
filter the dataset. The target is to select a subset of tracks that 
fulfil some specific requirements and mix only 2 channels. 
   

# Filtering MedleyDB

'''python
import medleydb as mdb
import librosa

'''
  



# Reference

[1] R. Bittner, J. Salamon, M. Tierney, M. Mauch, C. Cannam and J. P. Bello, 
"MedleyDB: A Multitrack Dataset for Annotation-Intensive MIR Research", 
in 15th International Society for Music Information Retrieval Conference, Taipei, Taiwan, Oct. 2014.
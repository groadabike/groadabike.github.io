---
layout: post
title: ACOMUS1 Corpus
subtitle: An acoustic music corpus 
image: 
bigimg: /img/2017-06-03/santuario.jpg
tags: 
---

Hi,  
This time I want to talk you about the ACOMUS1 corpus which is a Musical corpus, which is based on acoustic cover versions of popular songs.
First af all, this database was designed and constructed for academic porpoises only. 
Any other use is strictly forbidden and please avoid to do it. 
Also, all the songs and it lyrics are property of the artist and all the credits belong to them. 
I just processed the songs in order to use in a research dissertation.

## Characteristics

The corpus is designed and constructed by with the following specifications: 

1. The songs are mainly interpreted by *Amateur Artists* and around 10% are known artists.
2. The database is separated in a balanced number of *Male* and *Female* artists. 
3. The interpretations have one accompaniment instrument and just some few cases have more than one.
4. The 80% of the database are accompanied by guitar and 20% by piano.

## Replicate the database

For legal considerations the database is distributed as a series of step that re-construct the database rather than distribute the audio raw files.
The corpus was organize using codification for COVER-ARTIST, ARTIST-SONG, ACCOMPANIMENT-INSTRUMENT and PITCH-TEMPO modifications used in a speech recognition project.
This information can be found in [Database Description's](database/id_descriptions) directory

### Annotation Files

Each song is properly separated by sentence segments. 
This information was saved in a JSON file per song and can be found in directory [Json Lyrics](annotation/json_lyrics).

### Cover Lyrics

The preprocessed lyrics are in [Lyrics](annotation/lyrics) directory.

### Replicate Current State of the Corpus

The current state of the corpus have 120 song annotated and segmented.
This process will be updated constantly.

The process download the raw-audio, create a 16KHz copy and extract each segment of speech.
Also, a text file is generated indicating the content of each segment.

For replicate the current state of the database run **run_current.sh** file from **current_state** directory.

1. Check the **path.sh** file and adjust it to the user paths.
2. Run run_current.sh for the whole process.

### Create and Edit a Song

If the user want to, it is possible to modify, add or eliminate songs and annotation files.
Refer to readme file in [Acomus Corpus](acomus_corpus) directory for the description from scratch of each step. 
The current state of this description is **IN PROCESS**.

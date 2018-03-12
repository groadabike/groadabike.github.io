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

We are going to create a subset of MedleyDB with tracks that fulfil:

1. Track with vocal
2. Track with acoustic guitar

Firstly, we need to know what instrument are available in the corpus.
Then, we can select the desired instrument.

The next code shows us a set with all of the labeled instrument in MedleyDB. 
```python
import medleydb as mdb

instruments = mdb.get_valid_instrument_labels()
print(instruments)  

```

Now we are going to concentrate only in the vocals instrument labeled as 
**<gender> singer** and we are going to print out the ranking of the
STEM and the ID of the track.
All the vocal instrument that we want will be saved in *instruments* list 


```python
import medleydb as mdb

instruments = ['female singer', 'male singer']
mtrack_generator = mdb.load_all_multitracks()
for mtrack in mtrack_generator:
    track = mdb.MultiTrack(mtrack.track_id)
    for key, stem in track.stems.items():
        if stem.instrument[0] in instruments:
            print(stem.ranking, mtrack.track_id)

```

Now, we are going to add a second list of instruments for the accompaniment
background called *back_instruments*, and we will print out the audio 
path of each of the tracks.
It is mandatory that the **MEDLEYDB_PATH** variable is properly set
in order to this code works ([python tools](https://github.com/marl/medleydb)).   

```python
import medleydb as mdb

instruments = ['male singer', 'female singer']
back_instruments = ['acoustic guitar']

mtrack_generator = mdb.load_all_multitracks()
for mtrack in mtrack_generator:
    track = mdb.MultiTrack(mtrack.track_id)
    for key, stem in track.stems.items():
        if stem.instrument[0] in instruments:
            if stem.ranking == 1:
                print(stem.audio_path)

        if stem.instrument[0] in back_instruments:
            print(stem.audio_path)
```

Finally, using [libROSA](https://librosa.github.io/librosa/)[2] python module
we are going to create a new mix file that only contain the vocal channel and
acoustic guitar accompaniment.

In order to create this new mix-audio, we are going to save tha audio-path
of each track in a python list.
In this same list, in index ZERO, we are going to save the track_ID.

```python
import medleydb as mdb
import librosa

instruments = ['male singer', 'female singer']
back_instruments = ['acoustic guitar']

mixtrack=[] 
track_name = ''
mtrack_generator = mdb.load_all_multitracks()
for mtrack in mtrack_generator:
    mixtrack = []
    track = mdb.MultiTrack(mtrack.track_id)
    mixtrack.append(track.track_id)
    for key, stem in track.stems.items():
        if stem.instrument[0] in instruments:
            if stem.ranking == 1:
                mixtrack.append(stem.audio_path)

        if stem.instrument[0] in back_instruments:
            mixtrack.append(stem.audio_path)

    if len(mixtrack) > 2:
        result = None
        sr_out = 0
        track_name = mixtrack.pop(0)
        print(track_name)
        for mt in mixtrack:
            print(mt)
            y, sr = librosa.load(mt, sr=None)
            if result is None:
                result = y
                sr_out = sr
            else:
                result = result + y

        librosa.output.write_wav(track_name + '.wav', result, sr_out)

```   
Currently, this example results 15 track with vocal with stem's ranking
number 1 and with acoustic guitar as one of the accompaniment instruments.   

Hope this small example helps you to work with MedleyDB.
I was not expected to offer an error-proof python code but a simple
example of how it is possible to create a mix-audio with a subset of 
track's components.



# Reference

[1] R. Bittner, J. Salamon, M. Tierney, M. Mauch, C. Cannam and J. P. Bello, 
"MedleyDB: A Multitrack Dataset for Annotation-Intensive MIR Research", 
in 15th International Society for Music Information Retrieval Conference, Taipei, Taiwan, Oct. 2014.

[2] McFee, Brian, Colin Raffel, Dawen Liang, Daniel PW Ellis, Matt McVicar, Eric Battenberg, 
and Oriol Nieto. "librosa: Audio and music signal analysis in python." In Proceedings of the 14th python 
in science conference, pp. 18-25. 2015.
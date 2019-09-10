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

MedleyDB [1]  is a great and free dataset for music research created at NYU’s Music 
and Audio Research Lab. This collection consists of 122 different multi-tracks of 44.1 kHz
wav files. Each of the components of the tracks is divided into an independent channel.
For a detailed description of the dataset, please refer directly to the 
[project website](http://medleydb.weebly.com/).

In addition to the dataset, on the website of the project is it possible to find a great 
set of [Python tools](https://github.com/marl/medleydb)
 that allows working with the entire collection efficiently.  
 
In this post, we will focus on the use of these tools to:
select a subset of tracks and re-mix two channels.
   

# Filtering MedleyDB

We are going to select a subset of MedleyDB with the tracks that:

1. Contain one or more vocal channel. 
2. Contain one or more acoustic guitar channel.

Firstly, we need to know what instrument are available in the corpus.
Then, we can select the desired instrument.

The next code shows us a set with all of the labelled instrument in MedleyDB. 
```python
import medleydb as mdb

instruments = mdb.get_valid_instrument_labels()
print(instruments)  

```

Now we are going to filter the channels with instrument labelled as 
**male/female singer** and we are going to print out the ranking of the
STEM and the ID of the track.


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

Now, we filter the tracks that labelled as "acoustic guitar".

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
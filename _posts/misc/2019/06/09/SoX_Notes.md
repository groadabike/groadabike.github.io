---
layout: post
title: SoX Notes
subtitle: 
image: 
bigimg: /img/2018-02-05/baby-penguin.jpg
tags: [SoX]
date: 2019-06-09
published: True
---


# Install SoX with vorbis from source

Install SoX in a Ubuntu computer with 'Sudo' privileges is fast and easy.
A simple `sudo apt install sox` will do the job.  
But, do it in a computer without sudo needs more steps,
 specially if is needed to handle FLAC or OGG Vorbis files.
 
The following steps will install SoX from source with OGG Vorbis libraries.

1- Install libogg  
2- Install libvorbis  
3- Install SoX  

## Install libogg

Download libogg installer from [http://www.linuxfromscratch.org/blfs/view/cvs/multimedia/libogg.html](http://www.linuxfromscratch.org/blfs/view/cvs/multimedia/libogg.html)

```
./configure --prefix=$HOME/apps/libogg \
            --disable-static 
            --docdir=$HOME/apps/libogg/share/doc/libogg-1.3.3
make
make install
```

## Install libvorbis

Download libvorbis from [http://www.linuxfromscratch.org/blfs/view/cvs/multimedia/libvorbis.html](http://www.linuxfromscratch.org/blfs/view/cvs/multimedia/libvorbis.html)

```
./configure --prefix=$HOME/apps/libvorbis 
            --disable-static 
            --with-ogg-libraries=$HOME/apps/libogg/lib 
            --with-ogg-includes=$HOME/apps/libogg/include
make
make install

```

## Install SoX

Download SoX from [http://sox.sourceforge.net/](http://sox.sourceforge.net/)


```
./configure --prefix=$HOME/apps/sox 
            LIBS="-L$HOME/apps/libogg/lib -L$HOME/apps/libvorbis/lib" 
            CPPFLAGS="-I$HOME/apps/libogg/include -I$HOME/apps/libvorbis/include"
make
make install
            
```


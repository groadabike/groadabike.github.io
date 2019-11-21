---
layout: post
title: Kaldi-nnet3
subtitle: Notes about the config file
image: 
bigimg: /img/2018-02-05/baby-penguin.jpg

date: 2019-11-21
published: True
---
### _Post under construction_

# Introduction

Kaldi ["nnet3"](https://kaldi-asr.org/doc/dnn3.html) is a robust framework for DNN acoustic modelling.
In almost all the recipes, you can find examples of different configuration that can be adapted to use it in your own task.
However, to understand how to adapt the xconfig file to implement more sophisticated (and not too sophisticated sometimes) ideas is not a process.

# nnet3 structure

Nnet3 neural network is constructed using a general graph structure consisting in:
* A list of Components
* A graph structure that specify how the Components are connected

The network construction is based-on a config file where the Components, nodes, inputs and outputs are defined.

TODO- add Index and Cindex descripton

# xconfig to config

The **xconfig** are simplified configuration files to define the structure of the network.
This files are parse by using the script steps/nnet3/xconfig_to_configs.py passing the xconfig file and output path.
e.g.  
```
config_dir=etc/chain/tdnn/configs
steps/nnet3/xconfig_to_configs.py --xconfig-file $config_dir/network.xconfig \
                                  --config-dir $config_dir/configs/ 
```

# Layers

This is an explanation how a line in xconfig is parse into the final configuration of the network.
Kaldi groups the layers into several kinds.
I will list all of the layers. But, I will only detail some of them.

* [basic_layers](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/basic_layers.py)
    * input
    * output (not real outputs, they just directly map to an output-node in nnet3)
    * output_layer (real output layer)
    * relu-renorm-layer (or any combination between relu, renorm, sigmoid amd tanh)
    * fixed-affine-layer
    * affine-layer 
    * idct-layer (to convert input MFCC-features to Filterbank features)
    * spec-augment-layer
* [convolution](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/convolution.py)
    * conv-batchnorm-layer
    * conv-renorm-layer
    * res-block (residual block as in ResNets)
    * res2-block (residual block with post-activations, with no support downsampling)
    * SumBlockComponent (For channel averaging)
* [attention](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/attention.py)
    * attention-renorm-layer
    * attention-relu-renorm-layer
    * attention-relu-batchnorm-layer
    * relu-renorm-attention-layer
    * or any combination of relu, attention, sigmoid, tanh, renorm, batchnorm, dropout
* [lstm](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/lstm.py)
    * lstm-layer 
    * lstmp-layer
    * lstmp-batchnorm-layer (followed by batchnorm)
    * fast-lstm-layer
    * fast-lstm-batchnorm-layer (followed by batchnorm)
    * lstmb-layer
    * fast-lstmp-layer
    * fast-lstmp-batchnorm-layer
* [gru](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/gru.py)
    * gru-layer (Gated recurrent unit)
    * pgru-layer (Personalized Gated Recurrent Unit)
    * norm-pgru-layer (batchnorm in the forward direction, renorm in the recurrence)
    * opgru-layer (Output-Gate Projected Gated Recurrent Unit) [paper](http://www.danielpovey.com/files/2018_interspeech_opgru.pdf)
    * norm-opgru-layer (batchnorm in the forward direction, renorm in the recurrence)
    * fast-gru-layer 
    * fast-pgru-layer
    * fast-norm-pgru-layer (batchnorm in the forward direction, renorm in the recurrence)
    * fast-opgru-layer
    * fast-norm-opgru-layer
* [stats_layer](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/stats_layer.py)
* [trivial_layers](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/trivial_layers.py)
* [composite_layers](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/composite_layers.py)

## Basic Layers


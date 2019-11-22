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

Kaldi ["nnet3"](https://kaldi-asr.org/doc/dnn3.html){:target="_blank"} is a robust framework for DNN acoustic modelling.
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

* [basic_layers](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/basic_layers.py){:target="_blank"}
    * input
    * output (not real outputs, they just directly map to an output-node in nnet3)
    * output_layer (real output layer)
    * relu-layer
    * relu-renorm-layer
    * relu-batchnorm-dropout-layer
    * relu-dropout-layer
    * relu-batchnorm-layer
    * relu-batchnorm-so-layer 
    * batchnorm-so-relu-layer
    * batchnorm-layer
    * sigmoid-layer
    * tanh-layer
    * fixed-affine-layer
    * affine-layer (fully connected layer)
    * idct-layer (to convert input MFCC-features to Filterbank features)
    * spec-augment-layer
* [convolution](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/convolution.py){:target="_blank"}
    * conv-batchnorm-layer
    * conv-renorm-layer
    * res-block (residual block as in ResNets)
    * res2-block (residual block with post-activations, with no support downsampling)
    * SumBlockComponent (For channel averaging)
* [attention](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/attention.py){:target="_blank"}
    * attention-renorm-layer
    * attention-relu-renorm-layer
    * attention-relu-batchnorm-layer
    * relu-renorm-attention-layer
    * or any combination of relu, attention, sigmoid, tanh, renorm, batchnorm, dropout
* [lstm](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/lstm.py){:target="_blank"}
    * lstm-layer 
    * lstmp-layer
    * lstmp-batchnorm-layer (followed by batchnorm)
    * fast-lstm-layer
    * fast-lstm-batchnorm-layer (followed by batchnorm)
    * lstmb-layer
    * fast-lstmp-layer
    * fast-lstmp-batchnorm-layer
* [gru](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/gru.py){:target="_blank"}
    * gru-layer (Gated recurrent unit)
    * pgru-layer (Personalized Gated Recurrent Unit)
    * norm-pgru-layer (batchnorm in the forward direction, renorm in the recurrence)
    * opgru-layer (Output-Gate Projected Gated Recurrent Unit) [paper](http://www.danielpovey.com/files/2018_interspeech_opgru.pdf){:target="_blank"}
    * norm-opgru-layer (batchnorm in the forward direction, renorm in the recurrence)
    * fast-gru-layer 
    * fast-pgru-layer
    * fast-norm-pgru-layer (batchnorm in the forward direction, renorm in the recurrence)
    * fast-opgru-layer
    * fast-norm-opgru-layer
* [stats_layer](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/stats_layer.py){:target="_blank"}
    * stats-layer (adds statistics-pooling and statistics-extraction components)
* [trivial_layers](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/trivial_layers.py){:target="_blank"}
    * renorm-component
    * batchnorm-component
    * no-op-component
    * delta-layer 
    * linear-component
    * combine-feature-maps-layer
    * affine-component
    * scale-component
    * offset-component
    * dim-range-component
* [composite_layers](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/composite_layers.py){:target="_blank"}
    * tdnnf-layer (factorized TDNN)
    * prefinal-layer

## Some definitions from deepai.org
* so: Scale and offset
* [batchnorm](https://deepai.org/machine-learning-glossary-and-terms/batch-normalization){:target="_blank"}:  Batch normalization
* [affine](https://deepai.org/machine-learning-glossary-and-terms/affine-layer){:target="_blank"}: Affine layer
* [sigmoid](https://deepai.org/machine-learning-glossary-and-terms/sigmoid-function){:target="_blank"}: Sigmoid function
* [relu](https://deepai.org/machine-learning-glossary-and-terms/rectified-linear-units){:target="_blank"}: Rectified linear units
* [gru](https://deepai.org/machine-learning-glossary-and-terms/gated-recurrent-unit){:target="_blank"}: Gated recurrent unit
* [lstm](https://deepai.org/machine-learning-glossary-and-terms/long-short-term-memory){:target="_blank"}: Long short-term memory 
* [attention](https://deepai.org/machine-learning-glossary-and-terms/attention-models){:target="_blank"}: Attention models
* [convolutional](https://deepai.org/machine-learning-glossary-and-terms/convolutional-neural-network){:target="_blank"}: Convolutional neural network


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
steps/nnet3/xconfig_to_configs.py --xconfig-file $config_dir/network.xconfig --config-dir $config_dir/configs/ 
```

# Layers

This is an explanation how a line in xconfig is mapped into the final configuration.
Kaldi groups the layers into several kinds.

* [Basic Layers](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/basic_layers.py)
* [convolution](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/convolution.py)
* [attention](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/attention.py)
* [lstm](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/lstm.py)
* [gru](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/gru.py)
* [stats_layer](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/stats_layer.py)
* [trivial_layers](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/trivial_layers.py)
* [composite_layers](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/composite_layers.py)



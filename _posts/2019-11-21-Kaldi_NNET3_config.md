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

TODO - 

# Layers

This is an explanation how a line in xconfig is mapped into the final configuration

## [Basic Layers](https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/basic_layers.py)

| Layers | xconfig | config |
|--------|:---------|:--------|
| input  | input name=ivector dim=100   | input-node name=ivector dim=100 |
|        |  input name=input dim=40     | input-node name=input dim=40 |
| output | output-layer name=output dim=1924 input=Append(-1,0,1) | component name=output.affine type=NaturalGradientAffineComponent input-dim=900 output-dim=1924  max-change=1.5 param-stddev=0.0 bias-stddev=0.0 |
|        |                                                        | component-node name=output.affine component=output.affine input=Append(Offset(lda, -1), lda, Offset(lda, 1))  |
|        |                                                        | component name=output.log-softmax type=LogSoftmaxComponent dim=1924  |
|        |                                                        | component-node name=output.log-softmax component=output.log-softmax input=output.affine  |
|        |                                                        | output-node name=output input=output.log-softmax objective=linear  |
|        | output name=output input=Append(-1,0,1)                |






<table>
    <thead>
        <tr>
            <th>Layers</th>
            <th>Description</th>
            <th>xconfig</th>
            <th>config</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="2">input</td>
            <td rowspan="2">No extra parameter</td>
            <td>input name=ivector dim=100</td>
            <td>input name=input dim=100</td>
            <td>input-node name=ivector dim=100</td>
            <td>input-node name=input dim=100</td>
        </tr>
    </tbody>
</table>

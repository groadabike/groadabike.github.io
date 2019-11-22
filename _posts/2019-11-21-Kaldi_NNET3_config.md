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

<details><summary>basic_layers</summary>
<a href="https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/basic_layers.py" target="_blank">basic_layer.py</a>
<ul>
     <li>input</li>
     <li>output (not real outputs, they just directly map to an output-node in nnet3) </li>
     <li>output_layer (real output layer) </li>
     <li>relu-layer </li>
     <li>relu-renorm-layer</li>
     <li>relu-batchnorm-dropout-layer </li>
     <li>relu-dropout-layer </li>
     <li>relu-batchnorm-layer </li>
     <li>relu-batchnorm-so-layer </li>
     <li>batchnorm-so-relu-layer </li>
     <li>batchnorm-layer </li>
     <li>sigmoid-layer </li>
     <li>tanh-layer </li>
     <li>fixed-affine-layer (is an affine transform that is supplied at network initialization time and is not trainable)</li>
     <li>affine-layer (fully connected layer)</li>
     <li>idct-layer (to convert input MFCC-features to Filterbank features)</li>
     <li>spec-augment-layer</li>
</ul> 
 </details>
 
<details><summary>convolution</summary>
<a href="https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/convolution.py" target="_blank">convolution.py</a>
<ul>
     <li>conv-batchnorm-layer</li>
     <li>conv-renorm-layer</li>
     <li>res-block (residual block as in ResNets)</li>
     <li>res2-block (residual block with post-activations, with no support downsampling)</li>
     <li>SumBlockComponent (For channel averaging)</li>
</ul> 
 </details>
 
 <details><summary>attention</summary>
 <a href="https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/attention.py" target="_blank">convolution.py</a>
 <ul>
      <li>attention-renorm-layer</li>
      <li>attention-relu-renorm-layer</li>
      <li>attention-relu-batchnorm-layer</li>
      <li>relu-renorm-attention-layer</li>
      <li>SumBlockComponent (For channel averaging)</li>
      <li>or any combination of relu, attention, sigmoid, tanh, renorm, batchnorm, dropout</li>
 </ul> 
  </details>

 <details><summary>lstm</summary>
 <a href="https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/lstm.py" target="_blank">lstm.py</a>
 <ul>
      <li>lstm-layer </li>
      <li>lstmp-layer</li>
      <li>lstmp-batchnorm-layer (followed by batchnorm)</li>
      <li>fast-lstm-layer</li>
      <li>fast-lstm-batchnorm-layer (followed by batchnorm)</li>
      <li>lstmb-layer</li>
      <li>fast-lstmp-layer</li>
      <li>fast-lstmp-batchnorm-layer</li>
 </ul> 
  </details>

<details><summary>gru</summary>
<a href="https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/gru.py" target="_blank">gru.py</a>
    <li>gru-layer (Gated recurrent unit)</li>
    <li>pgru-layer (Personalized Gated Recurrent Unit)</li>
    <li>norm-pgru-layer (batchnorm in the forward direction, renorm in the recurrence)</li>
    <li>opgru-layer (Output-Gate Projected Gated Recurrent Unit) <a href="http://www.danielpovey.com/files/2018_interspeech_opgru.pdf" target="_blank">paper</a></li>
    <li>norm-opgru-layer (batchnorm in the forward direction, renorm in the recurrence)</li>
    <li>fast-gru-layer</li>
    <li>fast-pgru-layer</li>
    <li>fast-norm-pgru-layer (batchnorm in the forward direction, renorm in the recurrence)</li>
    <li>fast-opgru-layer</li>
    <li>fast-norm-opgru-layer</li>
</details>

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


# How to..
The order to construct a network definition in kaldi is first, define the **network.xconfig** file.
 Second, parse the xconfig to config with **steps/nnet3/xconfig_to_configs.py** script. Then, run the **steps/nnet3/chain/train.py**.
This pipeline is assuming that all the features and egs files already exists.  

## The network.xconfig file construction

One way to construct the xconfig is inserting the lines into the network.xconfig file directly in the sh script file. In this way, you will be able to set the different parameters using variables, keeping the script organize and easy to modify.

```
dir=exp/chain/tdnn_sp
mkdir -p $dir/configs
cat <<EOF > $dir/configs/network.xconfig
  input ...
  ...
  output-layer ...
EOF
steps/nnet3/xconfig_to_configs.py --xconfig-file $dir/configs/network.xconfig \
                                  --config-dir $dir/configs/
```

## Input layer

In the Kaldi recipes, it is common that the dimension of the input layer is 40 MFCC and, be used as a fix value  for the **dim** parameter for the input layer.
But sometimes, you may have vectors with a different size. Therefore, you may want the dim to be a dynamic value.
The way to do this is getting the dimension of the features vector and passing that value to the input layer.

```
# Getting features vector dimension
feat_path=data/train_clean_sp_hires
feat_dim=`feat-to-dim scp:${feat_path}/feats.scp -`
....
# Defining xconfig
input dim=$feat_dim name=input
...
```

The most basic input layer in xconfig would be:
```
feat_dim=40
input dim=$feat_dim name=input
```
And is parse to an input node in final.config as:
```
input-node name=input dim=40
```

But, if you add want to add iVectors, you need to add:
```
input dim=100 name=ivector
input dim=40 name=input
fixed-affine-layer name=lda input=Append(-1,0,1,ReplaceIndex(ivector, t, 0)) affine-transform-file=foo/lda.mat
```
which will be expanded in final.config as:
```
input-node name=ivector dim=100
input-node name=input dim=40
component name=lda type=FixedAffineComponent matrix=foo/lda.mat
component-node name=lda component=lda input=Append(Offset(input, -1), input, Offset(input, 1), ReplaceIndex(ivector, t, 0))
``` 

# Terminology 
Some of the terms have a link to the definition on the deepai.org website.


* so: Scale and offset
* [batchnorm](https://deepai.org/machine-learning-glossary-and-terms/batch-normalization){:target="_blank"}:  Batch normalization
* [affine](https://deepai.org/machine-learning-glossary-and-terms/affine-layer){:target="_blank"}: Affine layer
* [sigmoid](https://deepai.org/machine-learning-glossary-and-terms/sigmoid-function){:target="_blank"}: Sigmoid function
* [relu](https://deepai.org/machine-learning-glossary-and-terms/rectified-linear-units){:target="_blank"}: Rectified linear units
* [gru](https://deepai.org/machine-learning-glossary-and-terms/gated-recurrent-unit){:target="_blank"}: Gated recurrent unit
* [lstm](https://deepai.org/machine-learning-glossary-and-terms/long-short-term-memory){:target="_blank"}: Long short-term memory 
* [attention](https://deepai.org/machine-learning-glossary-and-terms/attention-models){:target="_blank"}: Attention models
* [convolutional](https://deepai.org/machine-learning-glossary-and-terms/convolutional-neural-network){:target="_blank"}: Convolutional neural network
* [lda](https://towardsdatascience.com/light-on-math-machine-learning-intuitive-guide-to-latent-dirichlet-allocation-437c81220158){:target="_blank"}: Latent Dirichlet allocation


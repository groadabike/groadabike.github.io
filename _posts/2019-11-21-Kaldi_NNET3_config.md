---
layout: post
title: Kaldi-nnet3
subtitle: Notes about the xconfig file
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
<ul>
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
</ul>    
</details>

 <details><summary>stats_layer</summary>
 <a href="https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/stats_layer.py" target="_blank">stats_layer.py</a>
 <ul>
      <li>stats-layer (adds statistics-pooling and statistics-extraction components)</li>
 </ul> 
  </details>
  
 <details><summary>trivial_layers</summary>
 <a href="https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/trivial_layers.py" target="_blank">trivial_layers.py</a>
 <ul>
      <li>renorm-component</li>
      <li>batchnorm-component</li>
      <li>no-op-component</li>
      <li>delta-layer</li>
      <li>linear-component</li>
      <li>combine-feature-maps-layer</li>
      <li>affine-component</li>
      <li>scale-component</li>
      <li>offset-component</li>
      <li>dim-range-component</li>
 </ul> 
  </details>

 <details><summary>composite_layers</summary>
 <a href="https://github.com/kaldi-asr/kaldi/blob/master/egs/wsj/s5/steps/libs/nnet3/xconfig/composite_layers.py" target="_blank">composite_layers.py</a>
 <ul>
      <li>tdnnf-layer (factorized TDNN)</li>
      <li>prefinal-layer</li>
 </ul> 
  </details>


# How to..
The order to construct a network definition in kaldi is first, define the **network.xconfig** file.
 Second, parse the xconfig to config with **steps/nnet3/xconfig_to_configs.py** script. Then, run the **steps/nnet3/chain/train.py**.
This pipeline is assuming that all the features and egs files already exists.  

## The network.xconfig file construction

One way to construct the xconfig file is inserting the lines into the network.xconfig file directly from the run_net.sh script file. In this way, you will be able to set the different parameters using variables, keeping the script organized and easy to modify.


```
dir=exp/chain/tdnn_sp
mkdir -p $dir/configs

# Definition of the  the xconfig
cat <<EOF > $dir/configs/network.xconfig
  input ...
  ...
  output-layer ...
EOF
# Parse xconfig to final, init and ref configs
steps/nnet3/xconfig_to_configs.py --xconfig-file $dir/configs/network.xconfig \
                                  --config-dir $dir/configs/
```

## Input layer

In the Kaldi recipes, it is common that the dimension of the input layer is 40 MFCC. In some cases, this is a hard value for the **dim** parameter in the input layer definition.
But sometimes, you may want to experiment with vectors of different size. Therefore, it would be more convenient to have a dynamic value that automatically take the vector size.
You can get the vector size by calling feat-to-dim function as:

```
# Getting features vector dimension
feat_path=data/train_clean_sp_hires
feat_dim=`feat-to-dim scp:${feat_path}/feats.scp -`

# Using the feat_dim in xconfig
input dim=$feat_dim name=input
```

#### MFCC features 
The most basic input layer in xconfig would be defined with the layer **input**. It is important that set the name of this layer as input.
```
 input dim=40 name=input
```

#### MFCC + iVectors features
If you want to concatenate iVectors with the MFCC, you need to define another input layer called ivector and a **fixed-affine-layer**. In the following example, the notation inside of the Append function assumes that exists an input-layer named as *input*, and it will replace the -1,0,1 notation to input[-1], input[0], input[1].

```
 input dim=100 name=ivector
 input dim=40 name=input

 # please note that it is important to have input layer with the name=input
 # as the layer immediately preceding the fixed-affine-layer to enable
 # the use of short notation for the descriptor
 fixed-affine-layer name=lda input=Append(-1,0,1,ReplaceIndex(ivector, t, 0)) affine-transform-file=foo/lda.mat
```

#### Using Filterbanks 
Kaldi preffers to save MFCC features because are more condense than the filterbanks features. So, if you, for example, want to train a cnn-tdnn network, you need to transform the MFCC to filterbanks to train the CNN part. To avoid to store both kind of features, in Kaldi exist the **idct-layer** that converts the MFCC into Filterbanks.

```
 input dim=40 name=input
 idct-layer name=idct input=input dim=40 cepstral-lifter=22 affine-transform-file=$dir/configs/idct.mat
```
In the case of the CNN-TDNN example, the order of the layers is important. Probably, you should think about it as a convention.
```
 input dim=100 name=ivector
 input dim=40 name=input
 
# please note that it is important to have input layer with the name=input
 # as the layer immediately preceding the fixed-affine-layer to enable
 # the use of short notation for the descriptor
 fixed-affine-layer name=lda input=Append(-1,0,1,ReplaceIndex(ivector, t, 0)) affine-transform-file=$dir/configs/lda.mat
 idct-layer name=idct input=input dim=40 cepstral-lifter=22 affine-transform-file=$dir/configs/idct.mat
```


#### Multiview features
In some scenarios, you may want to add different levels of features, e.g. frame, utterance, speaker, recording party, so on..
To do this you can concatenate the features as:
```
TODO add the example
```  

#### Multi-task 
I will not explain here how to construct a multi-task learning but, Josh Meyer has a nice template you can follow. [https://github.com/JRMeyer/multi-task-kaldi](https://github.com/JRMeyer/multi-task-kaldi)

## [TDNN layers](https://www.danielpovey.com/files/2015_interspeech_multisplice.pdf)

The following is an example of a common tdnn definition from librispeech recipe.
```
relu_dim=725
num_targets=$(tree-info $tree_dir/tree |grep num-pdfs|awk '{print $2}')
learning_rate_factor=$(echo "print (0.5/$xent_regularize)" | python)

cat <<EOF > $dir/configs/network.xconfig
  input dim=100 name=ivector
  input dim=40 name=input

  # please note that it is important to have input layer with the name=input
  # as the layer immediately preceding the fixed-affine-layer to enable
  # the use of short notation for the descriptor
  fixed-affine-layer name=lda input=Append(-1,0,1,ReplaceIndex(ivector, t, 0)) affine-transform-file=$dir/configs/lda.mat

  # the first splicing is moved before the lda layer, so no splicing here
  relu-batchnorm-layer name=tdnn1 dim=$relu_dim
  relu-batchnorm-layer name=tdnn2 dim=$relu_dim input=Append(-1,0,1,2)
  relu-batchnorm-layer name=tdnn3 dim=$relu_dim input=Append(-3,0,3)
  relu-batchnorm-layer name=tdnn4 dim=$relu_dim input=Append(-3,0,3)
  relu-batchnorm-layer name=tdnn5 dim=$relu_dim input=Append(-3,0,3)
  relu-batchnorm-layer name=tdnn6 dim=$relu_dim input=Append(-6,-3,0)

  ## adding the layers for chain branch
  relu-batchnorm-layer name=prefinal-chain dim=$relu_dim target-rms=0.5
  output-layer name=output include-log-softmax=false dim=$num_targets max-change=1.5

  # adding the layers for xent branch
  # This block prints the configs for a separate output that will be
  # trained with a cross-entropy objective in the 'chain' models... this
  # has the effect of regularizing the hidden parts of the model.  we use
  # 0.5 / args.xent_regularize as the learning rate factor- the factor of
  # 0.5 / args.xent_regularize is suitable as it means the xent
  # final-layer learns at a rate independent of the regularization
  # constant; and the 0.5 was tuned so as to make the relative progress
  # similar in the xent and regular final layers.
  relu-batchnorm-layer name=prefinal-xent input=tdnn6 dim=$relu_dim target-rms=0.5
  output-layer name=output-xent dim=$num_targets learning-rate-factor=$learning_rate_factor max-change=1.5
EOF
  steps/nnet3/xconfig_to_configs.py --xconfig-file $dir/configs/network.xconfig --config-dir $dir/configs/


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
* [lda](https://kaldi-asr.org/doc/transform.html#transform_lda){:target="_blank"}: Linear Discriminat Analyis


# Documentation of Components
If you check the **final.config** file after parse the xconfig, you will see that several components are inserted. Many of then are implicit when the definition of the network.
1. The [NaturalGradientAffineComponent](http://kaldi-asr.org/doc/nnet-simple-component_8h_source.html#l00745) component is the *Natural Gradient for Stochastic Gradient Descent* described in [paper](http://www.danielpovey.com/files/2015_aistats_dnn.pdf).

2. The [LinearComponent](http://kaldi-asr.org/doc/nnet-simple-component_8h_source.html#l00862) represents a linear (matrix) transformation of its input, with a matrix as its trainable parameters.  It's the same as NaturalGradientAffineComponent, but without the bias term.

2. The [TdnnComponent](http://kaldi-asr.org/doc/classkaldi_1_1nnet3_1_1TdnnComponent.html#details) is a more memory-efficient alternative to manually splicing several frames of input.



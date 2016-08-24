---
layout:     post
title:      "RNNLM初探"
subtitle:   "Magic of RNN"
date:       2016-08-24
author:     "WangJialin"
tags:
    - linux
    
---

# Recurrent Neural Networks: Language Modeling


Given words$$$x_{1},x_{2},...,x_{t}$$$,a language model will predict the following word $x_{t+1}$ by modeling:

$P(x_{t+1}=v_{j}|x_{t},...,x_{1})$

which $v_{j}$ is a word in vocabulary.

$e^{(t)}=x^{(t)}L 

$h^{(t)}=sigmoid(h^{(t-1)}H + e^{(t)}I + b_{1})$
$y^{(t)}=softmax(h^{(t)}U + b2)$

$P(x_{t+1}=v_{j}|x_{t},...,x_{1})=\hat{y}_{j}^{(t)}$

$x^{(t)}\in R^{|V|}$ : one-hot row vector representing the index of the current word. 

$L\in R^{|V|*d}$ : word embeddings, 

$H\in R^{D_{h}*D_{h}}$ : the hidden transformation matrix

$I\in R^{d*D_{h}}$ : the input word to hidden transformation matrix

$b_{1} \in R^{D_{h}}$ : bias

$b_{2} \in R^{|V|}$ : bias

$|V|$ : the vocabulary size; $d$ : the word embedding size; $D_{h}$ : the hidden layer dimension

### Loss Function:
cross entropy :
$J^{(t)}(\theta)=CE(y^{(t)},\hat{y}^{(t)})=-\sum_{i=1}^{|V|}y_{i}^{(t)}log\hat{y}^{(t)}$

运行结果：
```python
Epoch 0
Start Training
Step 1451: Training Perplexity: 467.649200
Start Vailidation
Validation Perplexity : 319.398163
Epoch 1
Start Training
Step 1451: Training Perplexity: 262.916992
Start Vailidation
Validation Perplexity : 248.669388
Epoch 2
Start Training
Step 1451: Training Perplexity: 208.227081
Start Vailidation
Validation Perplexity : 216.744110
Epoch 3
Start Training
Step 1451: Training Perplexity: 178.157455
Start Vailidation
Validation Perplexity : 199.078506
Epoch 4
Start Training
Step 1451: Training Perplexity: 158.884155
Start Vailidation
Validation Perplexity : 188.131851
Epoch 5
Start Training
Step 1451: Training Perplexity: 145.123047
Start Vailidation
Validation Perplexity : 181.001602
Epoch 6
Start Training
Step 1451: Training Perplexity: 134.578384
Start Vailidation
Validation Perplexity : 176.113007
Epoch 7
Start Training
Step 1451: Training Perplexity: 126.104927
Start Vailidation
Validation Perplexity : 172.894455
Epoch 8
Start Training
Step 1451: Training Perplexity: 119.073372
Start Vailidation
Validation Perplexity : 170.840912
Epoch 9
Start Training
Step 1451: Training Perplexity: 113.097527
Start Vailidation
Validation Perplexity : 169.660690
Epoch 10
Start Training
Step 1451: Training Perplexity: 107.932823
Start Vailidation
Validation Perplexity : 169.157349
Epoch 11
Start Training
Step 1451: Training Perplexity: 103.404007
Start Vailidation
Validation Perplexity : 169.246658
Epoch 12
Start Training
Step 1451: Training Perplexity: 99.390564
Start Vailidation
Validation Perplexity : 169.766541
Epoch 13
Start Training
Step 1451: Training Perplexity: 95.798798
Start Vailidation
Validation Perplexity : 170.761749
Epoch 14
Start Training
Step 1451: Training Perplexity: 92.556839
Start Vailidation
Validation Perplexity : 172.058868
Epoch 15
Start Training
Step 1451: Training Perplexity: 89.611221
Start Vailidation
Validation Perplexity : 173.743546
```
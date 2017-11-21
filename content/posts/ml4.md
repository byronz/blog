---
title: "ML Week4 - Neural Network"
date: 2017-10-24T22:36:04-04:00
tags: [ML, AI]
---

---

## Brain

- auditory cortex
- somatosensory cortex


![neuro](https://upload.wikimedia.org/wikipedia/commons/thumb/1/10/Blausen_0657_MultipolarNeuron.png/1200px-Blausen_0657_MultipolarNeuron.png)

## NN Model

> **Neuro.IO**
>
> Dentrite => Axon
>
> input layer > hidden layer (intermediate layer) > output layer


all hidden layer nodes are called "activation units"

$$a_i^{(j)} = \text{"activation" of unit $i$ in layer $j$}$$
$$\Theta^{(j)} = \text{matrix of weights controlling function mapping from layer $j$ to layer $j+1$}$$

### Forward Propagation


using trained parameters $\theta_{n}$ we can predict the output by calculate the output layer value

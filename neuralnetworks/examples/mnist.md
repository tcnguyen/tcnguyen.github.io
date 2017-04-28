---
layout: page
title: Handwritten digits recognition with MNIST dataset
---
1. One layer (no hidden layer)
==> 92%

- [TensorFlow and Deep Learning without a PhD, Part 1 (Google Cloud Next '17)](https://youtu.be/u4alGiomYP4)

- [codelabs.developers.google.com](https://codelabs.developers.google.com/codelabs/cloud-tensorflow-mnist/#0)

not usable in production (imagine detecting zip code application)

2. 5 fully connected layers
- hidden layers: 200, 100, 60, 30
- sigmoid activation ==> worse than ReLu thus use ReLu
- 10000 iterations ==>98%!, but messy noisy loss curve! ==> going **TOO FAST** (constant learning rate)
- Use decay learning rate ==> loss goes to 0, training accuracy can get to 100%, but NOT for test accuracy (**OVERFITTING**)
- Fact about NN: need big dataset for training!
- Try dropout but did not improve the test accuracy, still overfitting ==> conclusion: our NN is inadequate, not capable of extracting the necessary information from data by its architecture.
- When flattening X into 1d vector, **shape information is lost**
- **98%**

3. Convolutional Networks
- People begin to use stride 2 instead of pooling layer (45') (simpler)
- Use dropout (75%) only in FC layers (most of parameters are there)
- **99.3%**

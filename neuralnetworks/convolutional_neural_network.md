---
layout: page
title: Convolutional Neural Network Notes
---
### Notes:
#### 1. Dimensions:
- Input layer: $(W \times H \times D)$
- $K$ filters $(F \times F \times D)$
- Stride $S$ and padding $P$

$\rightarrow$ Width & height of the output layer:

$$
W_{out} = (W−F+2P)/S+1. \\
H_{out} = (H-F+2P)/S + 1.
$$

$\rightarrow$ Shape of output layer:
$$(W_{out} \times H_{out} \times K)$$

$\rightarrow$ Number of parameters:
$$(F \times F \times D + 1) \times K$$

#### 2. Pooling
**Benefits of pooling:**
- Reduce the size of the input
- Prevent overfitting

**Max pooling:**
- Allow the neural network to focus on only the most important elements

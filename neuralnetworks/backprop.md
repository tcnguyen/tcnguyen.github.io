---
layout: page
title: Back Propagation
---
### Parameters

Suppose we have an activation $a = f(W.X + b)$ with:

$$
X =
\begin{bmatrix}
 x_1^{(1)} & x_1^{(2)} & ... & x_1^{(M)} \\
 x_2^{(1)} & x_2^{(2)} & ... & x_2^{(M)} \\   
 ... & ...&...& ... \\
 x_n^{(1)} & x_n^{(2)} & ... & x_n^{(M)}
 \end{bmatrix}

\hspace{0.5cm}

  a =
  \begin{bmatrix}
   a_1^{(1)} & a_1^{(2)} & ... & a_1^{(M)} \\
   a_2^{(1)} & a_2^{(2)} & ... & a_2^{(M)} \\   
   ... & ...&...& ... \\
   a_m^{(1)} & a_m^{(2)} & ... & a_m^{(M)}
   \end{bmatrix}
$$

and $n$ and $m$ are input dimension and hidden layer dimension, $M$ is batch size. Parameters matrices are:

$$
W =
\begin{bmatrix}
 W_{1,1} & W_{1,2} & ... & W_{1,n} \\
 W_{2,1} & W_{2,2} & ... & W_{2,n} \\   
 ... & ...&...& ... \\
 W_{m,1} & W_{m,2} & ... & W_{m,n}
 \end{bmatrix}

\hspace{0.5cm}

b =
\begin{bmatrix}
 b_1  \\
 b_2 \\   
 ...  \\
 b_m
\end{bmatrix}
$$


The loss function for the batch is $J(a) \in R$ and is normally the sum over loss on each example in the batch.

$$J(a) = \frac{1}{M}\sum_{k=1}^{M} loss(a^{(k)})$$

For back propagation, suppose we have $da = \frac{\partial{J}}{\partial{a}}$, we want to compute $dX = \frac{\partial{J}}{\partial{X}}, db = \frac{\partial{J}}{\partial{b}}, dW = \frac{\partial{J}}{\partial{W}}$. We define:

$$ z = W.X + b $$

of shape $(m \times M)$ and $df(z)$ the pointwise derivative of $f$ at $z$ which has the same shape.

### Formula
1. For $a = f(z)$ a pointwise function ($a$ and $z$ of shape $m \times M$), we have the following equation since the derivative is taken pointwise:

$$
dz = da * df(z)
$$

2. For $z = W*X+b$ then we have:
$$dX = W^{T}.dz$$
$$dW = dz.X^{T}$$
$$db = sum(dz,\ axis = 1)$$

### Numpy

```python
def backprop(da, cache):
  #values computed during forward pass
  X,z,a,... = cache

  # get formula for df at z.
  # example f = sigmoid ==> dfz = sigmoid(z)*(1-sigmoid(z)) = a(1-a)
  # example f = tanh ==> dfz = (1-tanh(z)²) = (1-a²)
  dfz = ...

  # backprop
  dz = da*dfz
  dX = np.dot(W.T, dz)
  dW = np.dot(dz,X.T)
  db = np.sum(dz, axis=1, keepdims = True)
```

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

### Formula:

$$
dX = W^{T} . (da*df(z))  \hspace{1cm} (n\times m).(m \times M) = (n \times M)
$$

$$
dW = (da*df(z)) . X^{T}  \hspace{1cm} (m \times M) .  (M\times n) = (m \times n)
$$


$$
db = sum(da*df(z),\ axis = 1)
$$

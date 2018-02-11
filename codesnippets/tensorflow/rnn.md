---
layout: page
title: RNN in TensorFlow
---

#### Multiple layer
```python
def build_cell(num_units, keep_prob):
    lstm = tf.contrib.rnn.BasicLSTMCell(num_units)
    drop = tf.contrib.rnn.DropoutWrapper(lstm, output_keep_prob=keep_prob)
    return drop

tf.contrib.rnn.MultiRNNCell([build_cell(num_units, keep_prob) for _ in range(num_layers)])
```

*Note:*
Previously with TensorFlow 1.0, you could do this
```python
tf.contrib.rnn.MultiRNNCell([cell]*num_layers)
```
TensorFlow 1.0 will create different weight matrices for all cell objects, but starting with TensorFlow 1.1 you actually need to create new cell objects in the list.

#### Dropout
```python
lstm = tf.contrib.rnn.BasicLSTMCell(num_units)
tf.contrib.rnn.DropoutWrapper(lstm, output_keep_prob=keep_prob)
```
#### Gradient Clipping
```python
tvars = tf.trainable_variables()
grads, _ = tf.clip_by_global_norm(tf.gradients(loss, tvars), grad_clip)
train_op = tf.train.AdamOptimizer(learning_rate)
optimizer = train_op.apply_gradients(zip(grads, tvars))
```

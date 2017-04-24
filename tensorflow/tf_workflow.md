---
layout: page
title: Basic TensorFlow workflow
---
### Initialization:
- Create $X = (N, d_1, d_2, d_3)$ using placeholder for batch size $N$
- Create variables $W$ and $b$ with initial values
- Initialize all variables using `tf.intialize_all_variables()`
```python
import tensorflow as tf

# None will become the batch size 100
X = tf.placeholder(tf.float32, [None, 28, 28, 1])
W = tf.Variable(tf.zeros([784, 10]))
b = tf.Variable(tf.zeros([10]))

init =  tf.intialize_all_variables()
```

### Model
- Create $Y = X*W + b$ (use `tf.reshape` to flatten $X$)
- Use placeholder for true labels $Y\_$
- Compute the **cross entropy** loss function
- Compute the accuracy within the batch

```python
Y = tf.nn.softmax(tf.matmul(tf.reshape(X, [-1, 784]), W) + b)
Y_ = tf.placeholder(tf.float32, [None, 10])

#loss function
cross_entropy = -tf.reduce_sum(Y_ * tf.log(Y))

#% of correct answers found in batch
is_correct =  tf.equal(tf.argmax(Y,1), tf.argmax(Y_,1))
accuracy =  tf.reduce_mean(tf.cast(is_correct, tf.float32))
```
### Training
- Create an **optimizer** (`tf.train.GradientDescentOptimizer(learning_rate)`) and call the `optimize(loss_fn)` function which take a loss function as argument.

```python
optimizer = tf.train.GradientDescentOptimizer(0.003)
train_step = optimizer.optimize(cross_entropy)
```

### Run
- Create Session and run `init`
- Use `feed_dict` to feed data to placeholder

```python
sess = tf.Session()
sess.run(init)

for i in range(1000):
  # Load batch of images and correct answers
  batch_X, batch_Y = mnist.train.next_batch(100)
  train_data = {X: batch_X, Y_: batch_Y}

  # train
  sess.run(train_step, feed_dict=train_data)

  # success?
  # TIP: should do this every 100 iterations
  a,c = sess.run([accuracy, cross_entropy], feed_dict=train_data)

```

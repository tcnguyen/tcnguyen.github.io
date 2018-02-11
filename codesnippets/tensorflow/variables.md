---
layout: page
title: TensorFlow Variable
---
#### Creation
```python
with tf.variable_scope("scope"):
  W = tf.get_variable("W", shape=(in_size, out_size),
                           initializer=tf.contrib.layers.xavier_initializer())
  b = tf.get_variable("b", shape=(out_size,),
                           initializer=tf.zeros_initializer())
```  

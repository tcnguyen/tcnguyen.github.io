---
layout: page
title: Character-wise RNN trained on Anna Karenina
---
#### Outline
- Encode text as a long array of integer.
- Batch structure, example for each batch of size (3xM)

 a1 a2 ... aM | ................... | ...................

 b1 b2 ... bM | ................... | ...................

 c1 c2 ... cM | ................... | ...................

 Split the text in to (Nx (M*K)) where K is the number of batches. N is batch size and M is the sequence length in each batch.

- Use **yield** function generator to return batches, each of shape NxM
- Target is shifted by one character, we can use first input character as the last target character for simplicity
- Create RNN cell with Dropout and MultiRNNCell for many layers
- One hot encoding inputs and targets
- For training, use **gradient clipping** for exploding gradients
- Use **dynamic_rnn** to run on inputs

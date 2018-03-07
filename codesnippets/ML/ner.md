---
layout: page
title: Name Entity Recognition
---

### NLTK
```python
import nltk
from nltk.tokenize import sent_tokenize, word_tokenize

# Tokenize the article into sentences: sentences
sentences = sent_tokenize(article)

# Tokenize each sentence into words: token_sentences
token_sentences = [word_tokenize(sent) for sent in sentences]

# Tag each tokenized sentence into parts of speech: pos_sentences
pos_sentences = [nltk.pos_tag(sent) for sent in token_sentences]

# Create the named entity chunks: chunked_sentences
chunked_sentences = nltk.ne_chunk_sents(pos_sentences, binary=True)

# Test for stems of the tree with 'NE' tags
for sent in chunked_sentences:
    for chunk in sent:
        if hasattr(chunk, "label") and chunk.label() == "NE":
            print(chunk)
```

### Spacy
- Diffrent entity types compared to nltk
- Informal language corpora ==> easily find entities in Tweets and chat messages
- https://spacy.io/api/annotation#named-entities
```python
import spacy

# Instantiate the English model: nlp
# optional arguments: tagger=False, parser=False, matcher=False for speed
nlp = spacy.load('en', tagger=False, parser=False, matcher=False)

# Create a new document: doc
# article = text
doc = nlp(article)

# Print all of the found entities and their labels
for ent in doc.ents:
    print(ent.label_, ent.text)
```

### Multilingual NER with polyglot
- Vectors for many languages

```python
from polyglot.text import Text
# Create a new text object using Polyglot's Text class: txt
txt = Text(article)

# Print each of the entities found
print(txt.entities)

# [I-PER(['Charles', 'Cuvelliez']), I-PER(['Charles', 'Cuvelliez']),
# I-ORG(['Bruxelles']), I-PER(['l’IA']), I-PER(['Julien', 'Maldonato']),
# I-ORG(['Deloitte']), I-PER(['Ethiquement']), I-LOC(['l’IA']), I-PER(['.'])]

# Create the list of tuples: entities
entities = [(ent.tag, ' '.join(ent)) for ent in txt.entities]
```

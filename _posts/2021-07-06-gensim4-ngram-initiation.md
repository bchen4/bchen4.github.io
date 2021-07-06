---
layout: post
title:  Time-series Visualization
date:   2021-07-06 14:40
categories: Tech
tags: Tech
author: 豆藏
---

* content
{:toc}

Gensim 4.0.0 starts to include fasttext function. In order to user customer-defined ngrams, need to figure out where to insert the function.

FastText class inherits Word2Vec, and FastTextKeyedVector inherits KeyedVector class.

To use fasttext functions, just do:
```
model = FastText()
model.build_vocab(corpus_file=file, ...)
model.train()
```




After calling build_vocab(), the word vectors and ngram vectors are initiated.

build_vocab() is a function for Word2Vec class and it does those things: (use a table to indicate if that function existed in w2v class or ft class (override))


|functions called | w2v|ft|
|---|---|---|
|scan_vocab()	|Y	|N|
|prepare_vocab()	|Y	|N|
|estimate_memory()	|Y	|Y|
|prepare_weight()|	Y	|N|
|prepare_weights.init_weights()|	Y|	N|
|prepare_weights.resize_vectors()	|Y	|Y|
|add_lifecycle_event() |Y	|Y|


For FastText instance, word vector init process is the same as the Word2Vec instance. The question is where do ngram vectors get initiated?

Finally figured out ngram vectors are initiated inside prepare_weight() function. During the resize_vectors() step, a FT instance will use FT function. And to avoid extra calculation, the ft.resize_vectors() use a ft.prep_vectors() function to go over the word list stored in FastTextKeyedVector.word_to_index, and calculate all the ngram then initiate the vectors.

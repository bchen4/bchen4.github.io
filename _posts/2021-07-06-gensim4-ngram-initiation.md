---
layout: post
title:  Gensim4 ngram init
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


For FastText instance, word vector init process is the same as the Word2Vec instance. The question is where do ngram related variables get initiated?

* self.bucket: number of buckets, int

* self.buckets_word: For each key (by its index), report bucket slots their subwords map to. list of np.array.

* self.vector_ngrams



Since in fasttext, the ngrams are stored in buckets, which mean there will be multiple ngrams share one bucket. But the number of bucket also defined the number of vectors that need to be initiated. Number of buckets is given by the user.

In prepare_weights(), there is a step called init_weights() which will call ft.resize_vectors(), and ft.resize_vectors() uses keyedvectors.prep_vectors(n_rows, vector_size), a generalized vector initialize function to assign random values to  self.vector_ngrams.

After that, ft.resize_vectors() will call ft.recalc_char_ngram_buckets() function,  to initialize self.buckets_word.
```
for i, word in enumerate(self.index_to_key):
    self.buckets_word[i] = np.array(
        ft_ngram_hashes(word, self.min_n, self.max_n, self.bucket),
        dtype=np.uint32,
   )
```
Finally, ft.adjust_vectors() is called to adjust vectors for each word: word_vector = mean( word_vectors + vectors of all ngrams of this word)




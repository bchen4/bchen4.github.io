---
layout: post
title:  认知计算和大数据分析
date:   2018-10-27 22:16
categories: Book
author: bchen
---

* content
{:toc}


看完一章之后就觉得过于综述开始一目十行。这个所谓的书评就当做是一些笔记吧。直接读了英文版，专业词汇在这里就不花时间翻译了。

用了前四章的篇幅来讲什么是Cognitive系统， NLP和大数据系统的重要性。


### Cognitive Computing

最核心的特征是：是个probablistic systerm 而不是deterministic.

1. 对于每一个输入，结论不是只有一个。书中将这些可能的结果定义为“hypothesis generation”

2. 结论之间的界限也非常模糊， 所以需要一个强大的证据库和搜索，模式匹配系统作为支持；

3. 访问，管理和根据上下文分析数据。

4. 每次增加数据，系统都会自动更新，学习到新的内容 （跟Data loop的概念很像）



### NLP

书里强调了NLP的重要性，把它细分成了好几部分：

1. Phonology: sound of a language （感觉可以用于语言分类，也许能用于baby language）

2. Morphology: structure of a word （拼写检查？）

3. Lexical Analysis: Connect each word with its corresponding dictionary meaning （句子含义理解的第一步）

4. Discouse: meaning associated with context and time. （例如在70年代的美国，人们认为抽烟对身体有益。所以如果是70年代相关的文本提到抽烟，就不是一个负面词。）

5. Pragmatics: the ability to understand context of how words are used. (可以跟grammar结合起来)


### 云计算，机器学习

讲了很多应用案例来说明云计算和机器学习的重要性。但是很多都只是提了名词并没有深入。考虑到书是2015写的，近几年AWS和AI的进展实在是太快。书中提到的概念感觉大家已经耳熟能详了。



### Waston

第九章具体讲了Waston的架构和一些实现功能的模块，对于理解这个系统设计非常有帮助。
![Waston DeepQA](https://github.com/bchen4/bchen4.github.io/blob/master/img/DeepQA-Arch.png)

这里也解答了“如果产生假设？”

我是理解就是前期海量资料收集和分类非常重要，基本就是在了解问题之后进行各种搜索。从不同的角度，从不同的资料库找到可能的答案作为“假设”。然后对每个假设，再次寻找证据来打分。

打分的方法提到了几个，书里没有讲具体算法，列举一下名字以后备查：

1. Passage term match

2. skip-bigram

3. textual alignment

4. Logical Form Answer Candiate Scores (LFACS)


##
同时也发表在[豆瓣日记](https://book.douban.com/review/9729439/)

## ChangeLog
20181027 初稿

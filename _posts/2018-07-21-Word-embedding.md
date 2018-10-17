---
layout: post
title: 理解Word Embedding
categories: NMT
description: 理解Word Embedding的前生今世
keywords: NMT, 机器翻译
---

最近在看几篇NMT相关的经典论文，其中word embedding多次出现，经过查阅相关资料和博客，谈谈自己对word embedding的理解。

## 为什么要word embedding？
在计算机的世界里，所有的计算都是以数值进行的，如图片中一个像素点可以由0-255表示，一张图片即对应一个矩阵。计算机如何去处理一段文字呢？首先涉及到文字转数字的过程，对于一段文字，可以由基本的单词（包括各种量词、名词、动词等）构成，所有的单词构成完整的词汇表，所有的句子构成了语料库。word embedding实现了单词转化为数值矢量的过程，并使其更好地应用于计算机处理中，如NLP。

## 未完...

## 后话


## 参考

* word2vec前世今生
<https://www.cnblogs.com/iloveai/p/word2vec.html>
* 浅谈word embedding
<https://blog.csdn.net/puredreammer/article/details/78330821>
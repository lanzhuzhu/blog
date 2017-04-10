---
title: Sentence Similarity Learning by Lexical Decomposition and Composition
date: 2017-04-10
tags:
  - QA
  - 语义匹配
categories:
  - 自然语言处理
---



## Sentence Similarity Learning by Lexical Decomposition and Composition

- ZhiguoWang and Haitao Mi and Abraham Ittycheriah
- IBM T.J. Watson Research Center
- Yorktown Heights, NY, USA
- 2016.2.23

### 摘要
传统句子相似性方法关注输入两个句子的相似部分，易忽略不相似部分。本文提出的模型，通过分解和组合词汇的语义考虑了句子的相似性和不相似性。在answer
sentence selection实现了最佳成绩。

### introduction
1. 【**背景意义**】计算句子相似性的重要性，举例，在NLP/IR 领域，在PI（paraphrase identification），QA，IR 任务
2. 【**问题挑战**】然而，相似性学习有如下问题：
> - 语义相似词汇不同
> - 语义相似性在不同粒度级别上的测量（词、短语、句法）
> - 两个句子的不同部分是重要线索，如何利用？

3. 【**已有研究及不足**】为了解决上述问题的已有研究，分别对三个问题回答：1）已经提出的基于知识和基于语料的词汇相似性测量方法 2）利用从n-grams，连续短语，不连续短语中抽取特征；3）利用句子间不同作PI任务，需要人工标注数据。【少研究，已有的不足。】
4. 【**本文工作**】 介绍系统几个步骤，解决什么问题
5. 【**文章组织**】

### 句子相似性模型

难点：句子的分解，计算每个句子的每个词和另一个句子对应的相关度公式5。然后根据**匹配的相关度**得到两个矩阵表示，自己这个句子的每个词，在另一个句子上最相关的语义表达，公式6，三种方法。公式7-9也记录了三种利用公式6，将原来输入的句子分解成相似和不相似的两部分，有严格匹配，线性和正交三种分解法。

![](https://ww2.sinaimg.cn/large/006tNbRwgy1fegsu96wf5j30r612c0uq.jpg)


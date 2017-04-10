---
title: {{  Sequence to Sequence Learning with Neural Networks }}
date: {{ 2017/03/05 }}
tags: 自然语言处理

---

## 1. Sequence to Sequence Learning with Neural Networks

DNN 不能用来map sequences to sequences.本文提出一个 一般的end-to-end方法可以去学习变长结构的序列。使用多层LSTM去匹配输入到一个固定维度向量，然后另一个deep LSTM从该中间向量解码出目标句子。

![s2sModel](https://ww4.sinaimg.cn/large/006tNbRwgy1fefi2t7sejj30m307kq36.jpg)

![s2sEq](https://ww1.sinaimg.cn/large/006tNbRwgy1fefi2z84uhj30j9020743.jpg)

LSTM的目标是估计出左边的最大概率。首先计算出固定维度表达V，表示 the input sequence $ (x_1,…,x_T)$ ,即LSTM的最后一个隐藏状态。每个右边子公式的分布是用词典里所有词的softmax表示。每个句子使用EOS结束符号，使得模型能够定义所有可能长度的句子分布。最大化log概率，训练目标：其中S是很多句子对的训练集。![s2sloss](/Users/vera\百度云同步盘\PHD\reread paper\pic\s2sloss.PNG)

**result :** 

![S2Sresult2](/Users/vera/百度云同步盘\PHD\reread paper\pic\S2Sresult2.PNG)

**conclusion:** 编码最大数量的短期依赖，使学习问题变得简单。反转源语言句子使结果提高很多，也是因为此。

---

## 2. aNMM: Ranking Short Answer Texts with Attention-Based  Neural Matching Model

#### 文章来源：

**CIKM’16** , October 24-28, 2016, Indianapolis, IN, USA

==Liu Yan==g 1  Qingyao Ai 1 Jiafeng Guo 2 W. Bruce Croft 1

1. Center for Intelligent Information Retrieval, University of Massachusetts Amherst, MA, USA


2.  CAS Key Lab of Network Data Science and Technology, Institute of Computing Technology, Chinese Academy of Sciences, Beijing, China

{lyang, aiqy, croft}@cs.umass.edu, guojiafeng@ict.ac.cn

题外话：本文的实现是基于java的，难怪最后模型是softmax那样的训练方法。https://github.com/yangliuy/aNMM-CIKM16 开源程序。

#### 要解决的问题和已有方法：

基于attention的神经匹配模型去排序短文本；数据集TREC QA。

之前模型，结合额外特征，如word overlap& BM25;没有这些特征，模型表现差于语言特征工程。

本文解决问题：RQ1：不结合额外特征，DL模型能实现或者超过使用特征工程的方法么？

RQ2：结合额外特征，本文模型能优于当前QA的最好模型么？

之前不足：

1. 之前模型的不止为QA匹配：CNN使用position-valued权重，LSTM问答对序列，没有直接交互不能获得有效匹配信号。
2. 缺少建模问题的关注点。

#### 本文方法和创新点

1. 采用value-shared weighting scheme 而不是 position-shared weighting来结合匹配信号，

2. 并且包含问题词重要性学习，通过问题attention 网络。

3. 拓展的实验评估和不错的结果。

   ![model2017-03-30 下午4.46.32](https://ww2.sinaimg.cn/large/006tNbRwgy1fefi32hfmgj31kw0w4wxi.jpg)

   从输入层到隐藏层的计算：valued-shared weighting，如何体现的？是否有意义？

   ![eq2017-03-30 下午4.48.23](https://ww4.sinaimg.cn/large/006tNbRwgy1fefi33cmi4j30f403wmxa.jpg)

   从隐藏层到输出层（还有一个softmax 门学习question权重）：question attention network,通过这个attention网络的输出 来 给定“  问答对匹配信号”的 权重  即这里的v，这里使用了nomalize，归一化。

   ![屏幕快照 2017-03-30 下午5.00.52](https://ww3.sinaimg.cn/large/006tNbRwgy1fefi34mgmtj30to050t9b.jpg)

   aNMM-2增加了$r_t$  T为隐藏向量节点数，图中多个黄色点。w为多个权重向量，二维矩阵。两个隐藏层，相比于上个公式，1）增加线性结合的结点输出，2）额外激活函数$\tau$ 。![屏幕快照 2017-03-30 下午5.57.32](https://ww4.sinaimg.cn/large/006tNbRwgy1fefi39bmd3j30p80583yz.jpg)

   推导反向传播的公式，损失函数含义？

   ​
#### 效果和评价

   可以看出ANMM1模型效果其实更好，而且加入了attention提高不多。和我biLSTM/cnn差不多。

   它的实验使用的==word embeding 是700维==(300-700)维较好，在验证集上测试，可以试试。

   ![屏幕快照 2017-03-30 下午8.49.11](https://ww3.sinaimg.cn/large/006tNbRwgy1fefi3bedcvj30ya110wol.jpg)

评价：attention 比较简单，就是加了一个可训练的权重v。至于value-shared weighting，说不上效果如何，有多大提高，本文使用number of bins=600，比较细粒度的对权重进行分类学习。问答对的匹配的信号范围[-1，1]，由cosine计算得到。如下图，说不上这个value-shared weighting有什么规律？！![屏幕快照 2017-03-30 下午9.09.59](https://ww1.sinaimg.cn/large/006tNbRwgy1fefi3cvnrzj30ya110wol.jpg)

## 3. Bilateral Multi-Perspective Matching for Natural Language Sentences

#### 文章来源

- Zhiguo Wang(王志国), Wael Hamza and Radu Florian. 2017.2.28   In eprint arXiv:1702.03814.
- IBM T.J. Watson Research Center 1101 Kitchawan Rd, Yorktown Heights, NY 10598 {zhigwang,whamza,raduf}@us.ibm.com

#### 要解决的问题和已有方法

==自然语言句子匹配==是很多任务的基础，之前的工作要么是单向的，要么只应用了一种粗糙（granular,eg,word-by-word ,sentence-by-sentence）的匹配，本文在三个任务上paraphrase identiﬁcation（PI）, natural language inference(自然语言推理) and answer sentence selection（AS）都取得了state-of-art成绩。

==Introduction:== NLSM(Natural language sentence matching)任务是……，在PI、推论、AS等任务中是基础。

NLSM任务中已提出两种DL框架，1.“Siamese” architecture，独立处理两个输入的句子，匹配决定基于两个句子向量。 优点是，共享参数使模型更小、易训练。缺点，编码过程无交互，损失信息。2. “Matching aggregation”框架，两个句子更小粒度匹配，然后集合匹配结果。然而之前的“MG”方法有限制：只探索了word-by-word 匹配，忽略了其他粒度匹配；匹配是单向的，忽略相反方向。

#### 本文的方法和创新点

给定两个句子P\Q，用biLSTM对他们分别编码，从两个方向P->Q,Q->P对其匹配。

在匹配过程中，从多视野的角度，一个句子的每一步都与另一个句子的所有time-steps对应匹配。然后，另一个BiLSTM被用来集合所有匹配结果到一个固定长度的向量。再连上一个全连接层得到匹配的结果。

##### 模型总览：

本文提出了BiMPM(bilateral multi-perspective matching)，从下图可以看出分为5个层，

![屏幕快照 2017-03-31 下午9.19.01](https://ww4.sinaimg.cn/large/006tNbRwgy1fefi3dqov0j31kw0fhdqa.jpg)

其中1：第一层说词表达层由两部分组成：word embeddings and character-composed embeddings（后者是把一个词的每个字符放到LST==M里？==pre-trained with GloVe [ Pennington et al., 2014 ] or word2vec [ Mikolov et al., ] .）

2. Context Representation Layer就是双向LSTM处理。

3. 匹配层。本文的关键创新点，多视角的匹配操作，具体见图2.

4. 第四层，结合层，就是用BiLSTM处理匹配之后的3的输出向量，这里采用最后一个时刻的向量进行连接，图中的四个绿色向量。

5. 预测层，两个前向神经网络处理固定长度的匹配向量（4）的输出，最后加一个softmax层分类输出。

##### 下面对于3进行详细解释：

   ​

   ![屏幕快照 2017-03-31 下午10.23.42](https://ww4.sinaimg.cn/large/006tNbRwgy1fefi3efja4j31kw0fhdqa.jpg)

   A).  图中四种不同视角的计算匹配方法，都是用了余弦匹配$f_m$来计算，返回向量$m = f_m(v_1,v_2;W)$ ，计算两个d维向量的匹配值，$W \in R^ {l \times d}$ ,可训练参数。

  返回向量$m=[m_1,…，m_k,…，m_l]$ ,其中$m_k = \cos (W_k \cdot v_1,W_K \cdot v_2)$ ，l表示视图的维度，这里的$W_k$是W的第k行。每个P的time-step生成一个向量，即连接了8个生成的视图向量，（正反向2*4种视角）。

   B). 图中四种视角只显示了单向P->Q, P中每个time-step与Q所有time-steps计算出一个m向量，总共实现双向。1）使用 Q的最后一个隐藏向量进行计算。![屏幕快照 2017-04-01 上午9.18.03](https://ww1.sinaimg.cn/large/006tNbRwgy1fefi3f1yzmj311i0vkgr5.jpg)

   2）max-pooling matching: 与q的每个time-step计算fm，再求最大值。![屏幕快照 2017-04-01 上午9.21.01](https://ww3.sinaimg.cn/large/006tNbRwgy1fefi3flfngj312s12ewjh.jpg)

   3）attentive-matching:计算与每个q的相关度权值，再加权求和，与这个含权值的向量计算$f_m$![屏幕快照 2017-04-01 上午9.23.24](https://ww4.sinaimg.cn/large/006tNbRwgy1fefi3g6chzj30qi06i0u2.jpg)

   ![屏幕快照 2017-04-01 上午9.35.42](https://ww2.sinaimg.cn/large/006tNbRwgy1fefi3i449dj30km08ejsl.jpg)

![屏幕快照 2017-04-01 上午9.36.38](https://ww4.sinaimg.cn/large/006tNbRwgy1fefi3jim9aj30m403yaav.jpg)

   4)max-attentive matching,如图所示，就是在3）计算出权重值$\alpha_{i,j}$的基础上，选出最高的cos相似值作为attentive向量，然后计算得$m_i = f_m (h^p_i,max(\alpha_{i,j}) )for j \in 1…N$.

##### 实验的设置

问题1：多视图中的l不是固定的8么？怎么设置成20？或者$l\in\{1,5,10,15,20\}$实验？

#### 效果和评价

实验结果：在三个任务上的实验结果证明了模型的有效性。早期的工作设计手工创建的特征，只能在某一个任务或数据集上work well,很难在其他任务上普遍很好。 DL的框架，第一种，Siamese architecture，忽视了较低level的交互特征是必不可少，后期，从很多层次的粒度来匹配句子的模型越来越多，本文就属于这种框架。如下也证明了该框架。

![屏幕快照 2017-04-01 上午11.34.43](https://ww2.sinaimg.cn/large/006tNbRwgy1fefi3n3nr7j31fe0qmqeb.jpg)

![屏幕快照 2017-04-01 上午11.36.42](https://ww4.sinaimg.cn/large/006tNbRwgy1fefi3of5kzj31fe0qmqeb.jpg)
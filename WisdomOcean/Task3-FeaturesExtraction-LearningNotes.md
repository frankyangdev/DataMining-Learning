### 1. Notebook ###

运行Notebook: [Task3-FeatureExtraction.ipynb](https://github.com/frankyangdev/DataMining-Learning/blob/main/WisdomOcean/Task3-FeatureExtraction.ipynb)

### 2. Word2Vec ###

在NLP中，要让计算机读懂文本语言，首先要对文本进行编码。常见的编码如独热编码（one-hot encoding），词袋模型（BOW，bag of words），词向量模型（word embedding）。而word2vec就是词向量模型中的一种，它是google在2013年发布的工具。


word2vec工具主要包含两个模型：连续词袋模型（CBOW，continuous bag of words）和跳字模型（skip-gram）。如下图所示，左边蓝色部分代表CBOW模型，右边绿色部分代表Skip-gram模型。它们两者的区别是，CBOW是根据上下文去预测目标词来训练得到词向量，如图是根据W(t-2),W(t-1),W(t+1),W(t+2)这四个词来预测W(t)；而Skip-gram是根据目标词去预测周围词来训练得到词向量，如图是根据W(t)去预测W(t-2),W(t-1),W(t+1),W(t+2)。根据经验，CBOW用于小型语料库比较适合，而Skip-gram在大型的语料上表现得比较好。

![image](https://user-images.githubusercontent.com/39177230/115414201-6b72a880-a228-11eb-9c6f-eecd3a5d3c54.png)


#### 2.1 CBOW模型 ####

* 输入层（Input layer）：目标单词上下文的单词（这里显示三个），每个单词用ont-hot编码表示，为[1 * V]大小的矩阵，V表示词汇大小；

* 所有的ont-hot矩阵乘以输入权重矩阵W，W是[V * N]大小的共享矩阵，N是指输出的词的向量维数；

* 将相乘得到的向量 （[1 * V] 的ont-hot矩阵乘上[V * N]的共享矩阵W） 相加，然后求平均作为隐层向量h， 大小为[1 * N]；

* 将隐层向量h乘以输出权重矩阵W'，W'是[N * V]大小的共享矩阵；

* 相乘得到向量y，大小为[1 * V]，然后利用softmax激活函数处理向量y，得到V-dim概率分布；

* 由于输入的是ont-hot编码，即每个维度都代表着一个单词，那么V-dim概率分布中，概率最大的index所指代的那个单词为预测出的中间词。

* 将结果与真实标签的ont-hot做比较，误差越小越好，这里的误差函数，即loss function一般选交叉熵代价函数。

以上为CBOW生成词向量的全过程。如果我们只是想提取每个单词的向量，那么只需要得到向量y就可以了，但训练过程中要去做预测并计算误差，去求得输入权重矩阵W和输出权重矩阵W'。

![image](https://user-images.githubusercontent.com/39177230/115414651-cc01e580-a228-11eb-8f66-1d1baef3487c.png)

```python

from gensim.models import word2vec
import logging
logging.basicConfig(format='%(asctime)s : %(levelname)s : %(message)s', level=logging.INFO)  # 输出日志信息
sentences = word2vec.Text8Corpus('text8')  # 将语料保存在sentence中
model = word2vec.Word2Vec(sentences, sg=1, size=100,  window=5,  min_count=5,  negative=3, sample=0.001, hs=1, workers=4)  # 生成词向量空间模型
model.save('text8_word2vec.model')  # 保存模型

```

1. sentences：可以是一个List，对于大语料集，建议使用BrownCorpus,Text8Corpus或·ineSentence构建。
2. sg： 用于设置训练算法，默认为0，对应CBOW算法；sg=1则采用skip-gram算法。
3. size：是指输出的词的向量维数，默认为100。大的size需要更多的训练数据,但是效果会更好. 推荐值为几十到几百。
4. window：为训练的窗口大小，8表示每个词考虑前8个词与后8个词（实际代码中还有一个随机选窗口的过程，窗口大小<=5)，默认值为5。
5. alpha: 是学习速率
6. seed：用于随机数发生器。与初始化词向量有关。
7. min_count: 可以对字典做截断. 词频少于min_count次数的单词会被丢弃掉, 默认值为5。
8. max_vocab_size: 设置词向量构建期间的RAM限制。如果所有独立单词个数超过这个，则就消除掉其中最不频繁的一个。每一千万个单词需要大约1GB的RAM。设置成None则没有限制。
9. sample: 表示 采样的阈值，如果一个词在训练样本中出现的频率越大，那么就越会被采样。默认为1e-3，范围是(0,1e-5)
10. workers:参数控制训练的并行数。
11. hs: 是否使用HS方法，0表示不使用，1表示使用 。默认为0
12. negative: 如果>0,则会采用negativesamp·ing，用于设置多少个noise words
13. cbow_mean: 如果为0，则采用上下文词向量的和，如果为1（default）则采用均值。只有使用CBOW的时候才起作用。
14. hashfxn： hash函数来初始化权重。默认使用python的hash函数
15. iter： 迭代次数，默认为5。
16. trim_rule: 用于设置词汇表的整理规则，指定那些单词要留下，哪些要被删除。可以设置为None（min_count会被使用）或者一个接受()并返回RU·E_DISCARD,uti·s.RU·E_KEEP或者uti·s.RU·E_DEFAU·T的函数。
17.sorted_vocab： 如果为1（defau·t），则在分配word index 的时候会先对单词基于频率降序排序。
18.batch_words：每一批的传递给线程的单词的数量，默认为10000

































### Reference: ###

1. [word2vec的原理及实现](https://blog.csdn.net/qq_30189255/article/details/103049569)

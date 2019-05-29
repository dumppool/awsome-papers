# Awsome deep-learning papers

我们习惯了去看各种各样的博客、教程、公众号去了解深度学习的知识。
比如「一篇文章带你理解Attention机制和Transformer」，
「什么是RNN、LSTM和GRU？」，「5分钟理解卷积神经网络」。

我们相信，这些内容会比论文读起来更为通俗易懂。

然而在我读了许多论文后发现，其实论文叙述问题的方式非常简洁。
阅读论文来学习一个Idea的方式非常高效。

这个项目旨在鼓励大家通过阅读论文的方式来学习深度学习的知识。
项目的方式是：对于每一个知识点，给出对应的论文以供阅读。

我们相信论文能够比科普性质的博客文章能更高效地帮助你理解相应的知识点。

## 自然语言处理中的attention机制

### [NEURAL MACHINE TRANSLATION BY JOINTLY LEARNING TO ALIGN AND TRANSLATE](https://arxiv.org/abs/1409.0473)

第一次将attention机制运用在自然语言处理（机器翻译）上。全文只提到过3次「attention」这个词，还是在同一段。文章将attention机制作为解决机器翻译的对齐问题的一种方式。
非常简洁地介绍了attention机制之前的rnn-encoder-decoder模型。
和使用attention机制后的rnn-encoder-decoder模型。

看完基本就理解自然语言处理中的attention机制是怎么回事了

### [Effective Approaches to Attention-based Neural Machine Translation](https://arxiv.org/abs/1508.04025)

在自然语言处理（还是机器翻译）中提出了两种有效的attention方法：global attetnion和local attention。其中global attention类似上篇文章。local attention要复杂一些，建议阅读时以弄明白global attention为主。

看完后会对attention机制的encoder-decoder模型有更为深入的理解。

### [Attention Is All You Need](https://arxiv.org/abs/1706.03762)

提出了transformer，一个完全使用attention机制（没有rnn了）的encoder-decoder模型。相信不少人选择去学习attention机制的目的都是为了弄明白transformer。

这篇论文对attention机制的介绍相比前两篇要抽象（非贬义）得多：

> An attention function can be described as mapping a query and a set of key-value pairs to an output, where the query, keys, values, and output are all vectors. The output is computed as a weighted sum of the values, where the weight assigned to each value is computed by a compatibility function of the query with the corresponding key.

如果在没有基础的情况下直接阅读这篇论文，可能会感觉吃力。

看完后，就可以自己手写一篇《一篇文章带你理解Attention机制和Transformer》了。
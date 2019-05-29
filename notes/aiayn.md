# Attention Is All You Need 阅读笔记

## 位置编码

> Since our model contains no recurrence and no convolution, in order for the model to make use of the order of the sequence, we must inject some information about the relative or absolute position of the tokens in the sequence. To this end, we add "positional encodings" to the input embeddings at the bottoms of the encoder and decoder stacks. The positional encodings have the same dimension dmodel as the embeddings, so that the two can be summed. There are many choices of positional encodings, learned and fixed.
> 
> In this work, we use sine and cosine functions of different frequencies:
> 
> $PE_{(pos,2i)} =sin(pos/10000^{{2i}/d_{model}})$
> 
> $PE_{(pos,2i+1)} =cos(pos/10000^{{2i}/d_{model}})$
> 
> where pos is the position and i is the dimension. That is, each dimension of the positional encoding corresponds to a sinusoid. The wavelengths form a geometric progression from 2π to 10000 · 2π. We chose this function because we hypothesized it would allow the model to easily learn to attend by relative positions, since for any fixed offset k, $PE_{pos+k}$ can be represented as a linear function of $PE_{pos}$.

乍一看，免不了疑惑：

* 这个编码公式是怎么想出来的？
* 为啥要这样，有啥好处？

下面就来解答这两个疑惑。

**这个位置编码，就跟你的手表（秒针、分针、时针）对时间的编码是一模一样的。**

对于一天中的某一个时刻$t$，你的手表会给出3个坐标，分别是秒针$(x_s, y_s)$、分针$(x_m, y_m)$、时针$(x_h, y_h)$，把它们连在一起，就构成了这个时刻$t$的位置编码：$(x_s, y_s, x_m, y_m, x_h, y_h)$。其中$d_{model}=6$。

明白了吗？

对于论文的编码：

> $PE_{(pos,2i)} =sin(pos/10000^{{2i}/d_{model}})$
> 
> $PE_{(pos,2i+1)} =cos(pos/10000^{{2i}/d_{model}})$


我们把分量$(PE_{(pos, 2i)}, PE_{(pos, 2i+1)})$一起考虑，并将这个二维向量用一个复数表示，记作$PE^2_{pos, i}$，不难看出：

$$
PE^2_{pos, i}=e^{i\frac{pos}{1000^{2i/d_{model}}}}
$$

这就是一个随$pos$转动的单位二维向量呀，而且转动的速度反比于分母，即越后面的维度，随$pos$转动的速度就越慢。

你可以将前两个维度看作手表的「秒针」，接下来的两个维度看作「分针」，再往后的两个维度看作「时针」，手表到这里就结束了（一共$3 \times 2 = 6$个维度），而这个位置编码则会继续下去，还有比时针转得更慢的针，直到$d_{model}$个维度（$\frac{d_{model}}{2}$根针）为止，所有这些针的二维坐标连接在一起就构成了这个位置编码。

我猜测作者大概率是从时钟获得启示而想到的这个编码方式。如果说这个编码方式完全没有从时钟3针对时间的编码方式中获得启示，我是不信的。

论文中说，这个编码方式使得：

> $PE_{pos+k}$ can be represented as a linear function of $PE_{pos}$

乍一看可能不明白这句话想表达什么。

对于每个针（二维分量）而言，从$pos$到$pos + k$无非就是转动一个角度，这个转动的角度只和$k$有关，和$pos$无关。转动当然是一个线性变换啊，也就是说$PE_{pos + k} = L_k \times PE_{pos}$，而$L_k$是一个矩阵，这个矩阵只和$k$（相对距离）有关。

这说明什么？这个编码方式使得编码后向量的相对距离和绝对位置无关呀。这就是这句话想表达的，即：

> We chose this function because we hypothesized it would allow the model to easily learn to attend by relative positions
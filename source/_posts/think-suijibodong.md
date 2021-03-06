---
title: 金融市场是否随机波动
date: 2021-06-27 21:14:01
tags: [思考,随机波动]
categories: [思考交易]
---

当我们说”随机波动“的时候<!-- more --> ，需要知道一个随机变量可以服从任意一种分布和随机过程，不是说他不是随机游走，50/50或者大样本下正态就不是“随机”的。

至于是不是随机的，我们可以认为，如果服从对某种分布拟合的很好，假设检验通过了，可以认为统计上就服从这种分布。当然这个分布（过程）可能有“飘移”（你们说的趋势），可以有（非零）偏度，可以有（大于3）峰度，这些统计特征可以帮助我们在即使是随机的情况下，也获得统计上的收益。

但是彻底回答这个问题有一个非常困难的问题，就是一个序列其实在每一时间点上都有一个对应信息集，而每个时间点上我们只能获得一个样本。所以我们必须把增量或者变化量本身作为（总体）。而他们不可能是同分布的，所以我们不得不对他们的演化方式做出一些假设才能进行进一步拟合（也就是随机过程和时间序列）。当然这个序列可以具有记忆性，使得我们拟合的这些样本不显得那么独立

而下文提到，实证中，这样的记忆性知有到二阶序列才会体现，所以对收益这一级，我们大可以采用马尔可夫的假设（到了波动率就不行了）。

在一般人的印象中，记忆性也许意味着“不那么随机”，所以才会有了一堆“波动率可预测”这类的说法。但也正如原文最后一句，我们不妨放弃“随不随机”和“预测”这种二元论，而进一步去研究统计上的性质。

————————————————————

很多答案，把“随机波动”定义为了大样本下符合正态分布和随机游走。问题是，真的只有这两种随机波动嘛，很显然不是的。

“随机波动”还是一个比较笼统的概念，具体如何随机波动则是一个值得深究的问题，因为丢硬币只是一个二元的简单情况。 显然，随机游走，仅仅是无数种“随机波动”当中最简单的一种

首先要知道的常识是因为价格本身存在着单元根等一系列糟糕的计量特性，人们一般建模收益（或者对数收益）。大量的实证结果表明收益存在以下“典型事实”：

1.长时间均值为0

2.几乎没有序列相关，即使有，acf消减的十分快（一般是一天）

3.经常具体负偏度和高峰度

然而即使是这样，对于“怎样波动”这个问题还是不能回答全面。因为上述特征的描述似乎能完全被一个偏t分布描述，而实际上是远远不能的。原因在哪？

1.虽然实证结果表明收益几乎没有序列相关，但是他的二阶”波动率“是存在明显序列相关的，而且体现出长记忆的特征

2.收益经常具有跳跃性，同时他的二阶”波动率“也具有跳跃性，这两个跳跃性还具有一定相关性

这里波动率打了引号，因为在发生1的问题下，”波动率“很可能连定义都没有

对于那些一门心思想”预测“的人来说，在这么多变态的不确定信息下还有哪些是确定的呢？

一条路径是就是被各家来回把弄的多因子，试图用一些财务和市场信息等其他因素解释资产的不同收益部分

令一条路径是就是利用”波动率“长记忆的特性，直接去”预测“波动率”，毕竟长记忆嘛，看起来也的确更好“预测”那么一些

期货则是一个更加特殊的东西，当人们”预测来预测去“这东西的时候，很多人忽略了他跟债券一样是个有期限结构的东西。因此期货的波动率从不是一个序列而是一个面，是一个更加多元的复杂东西

但是私以为，在如此一个不确定性占主导，而确定性只占少数的环境里。无论怎么”预测“，能做的都只是一小部分。更多的则是不同资产的组合能不能减小不确定性，资产本身的结构对不确定性的影响和对风险（也就是不确定性）的控制才是我们更应该关心的

只可惜这些更应该关心的东西，一心“预测”的人是看不到的，反而还觉得这些都是“理论”。借用Taleb的话：“All we know is we dont know"
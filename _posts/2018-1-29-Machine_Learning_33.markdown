---
layout: post
title:  机器学习（三十三）——机器学习的算法体系&相关术语表, ACBM算法, 高斯过程回归, Optimizer进阶
category: ML 
---

# t-SNE

## t-SNE（续）

![](/images/img2/t-SNE.png)

![](/images/img2/Sammon.png)

上图分别是使用t-SNE和Sammon mapping可视化MNIST数据集后的效果图。从中可以看出t-SNE图中，数据更成团状，可视化效果更好。

t-SNE的不足主要有四个:

>主要用于可视化，很难用于其他目的。比如测试集合降维，因为他没有显式的预估部分，不能在测试集合直接降维；比如降维到10维，因为t分布偏重长尾，1个自由度的t分布很难保存好局部特征，可能需要设置成更高的自由度。

>t-SNE倾向于保存局部特征，对于本征维数(intrinsic dimensionality)本身就很高的数据集，是不可能完整的映射到2-3维的空间

>t-SNE没有唯一最优解，且没有预估部分。如果想要做预估，可以考虑降维之后，再构建一个回归方程之类的模型去做。但是要注意，t-sne中距离本身是没有意义，都是概率分布问题。

>训练太慢。有很多基于树的算法在t-sne上做一些改进。

## FastTSNE

该工具包提供了两种快速实现tSNE的方法：

Barnes-hut tsne：源于Multicore tSNE，适用于小规模数据集，时间复杂度为O(nlogn)。

Fit-SNE：源于Fit-SNE的C++实现方法，适用于样本量在10,000以上的大规模数据集，时间复杂度为O(n)。

代码：

https://github.com/pavlin-policar/fastTSNE

## 参考

https://www.zhihu.com/question/52022955

t-sne数据可视化算法的作用是啥？为了降维还是认识数据？

https://mp.weixin.qq.com/s/Rs9ri6Xs5R-yitrda8pJMg

详解可视化利器t-SNE算法：数无形时少直觉

https://mp.weixin.qq.com/s/_DXMlNZHVKm2jMnLGQFM_Q

还在用PCA降维？快学学大牛最爱的t-SNE算法吧

http://www.datakit.cn/blog/2017/02/05/t_sne_full.html

t-SNE完整笔记

https://yq.aliyun.com/articles/70733

比PCA降维更高级——（R/Python）t-SNE聚类算法实践指南

https://mp.weixin.qq.com/s/7Vy7l1YyBT7rMYW2i1AsuA

线性判别分析(LDA)原理详解

https://mp.weixin.qq.com/s/cnzQ7XepftDOZXslCf1MUA

你真的会用t-SNE么？有关t-SNE的小技巧

https://mp.weixin.qq.com/s/lbpe2NO1m8S38wpnp47BEg

通过可视化隐藏表示，更好地理解神经网络

https://mp.weixin.qq.com/s/F08aOjKsVdRInN6GPNJ7cA

t-SNE：最好的降维方法之一

https://mp.weixin.qq.com/s/qHOmUJgalvxwEBCBiiR7ig

t-SNE

# 机器学习的算法体系&相关术语表

## 算法体系

![](/images/article/ML_2.png)

原文：

https://mp.weixin.qq.com/s/HapJwwmN3-dbQvzp2jzt1w

一文看懂机器学习的算法体系

## 相关术语表

https://mp.weixin.qq.com/s/6KRNawRE1i8de3ctgv6aVg

机器学习词汇表：纵览机器学习基本词汇与概念

https://mp.weixin.qq.com/s/AtImcIBaRIJVXpOP2hruBQ

Google发布机器学习术语表 (包括简体中文)

# ACBM算法

ACBM算法是在AC（Aho-Corasick）自动机（UNIX上的fgrep命令使用的就是AC算法）的基础之上，引入了BM（Boyer-Moore）算法的多模扩展，实现的高效的多模匹配。和AC自动机不同的是，ACBM算法不需要扫描目标文本串中的每一个字符，可以利用本次匹配不成功的信息，跳过尽可能多的字符，实现高效匹配。

>注： Alfred Vaino Aho，1941年生，加拿大计算机科学家。普林斯顿大学博士，长期供职于贝尔实验室，后为哥伦比亚大学教授。egrep和fgrep的发明人，AWK语言的联合发明人。著有《Principles of Compiler Design Compilers: Principles, Techniques, and Tools》。该书由于封面上有龙图案，又被称为“龙书”，是编译原理方面的权威书籍。2003年获IEEE John von Neumann Medal。

>Margaret John Corasick，贝尔实验室研究员。

>Robert Stephen Boyer，德克萨斯大学教授。

>J Strother Moore，德克萨斯大学教授。Boyer的好朋友，两人的绝大多数成就都是合作完成的。

参见：

http://blog.csdn.net/sealyao/article/details/6817944

ACBM算法

https://mp.weixin.qq.com/s/yVOgAuD9hEYAMdLyvae2VA

最长公共子序列与最长公共子串

https://mp.weixin.qq.com/s/8rP3fF9iVnplhkFmxuj6fw

一文读懂KMP算法

https://mp.weixin.qq.com/s/if7P0yq59DbBEjW15vfaQA

推荐一个高效算法wumanber：每秒680万匹配！

https://mp.weixin.qq.com/s/4m1O5ZHsZTRc-JuBF97_3w

字符串匹配的Boyer-Moore算法

# 高斯过程回归

从大的分类来说，机器学习的算法可分为两类：

1.定义一个模型，用训练数据训练模型的参数，然后用训练好的模型进行预测。这种方法的缺点在于，预测效果和模型与样本的匹配程度有关。比如对非线性样本采用线性模型，其预测效果通常不会太好。但是增加模型的复杂度，又会导致过拟合。

2.定义一个函数分布，赋予每一种可能的函数一个先验概率，可能性越大的函数，其先验概率越大。但是可能的函数往往为一个不可数集，即有无限个可能的函数，随之引入一个新的问题：如何在有限的时间内对这些无限的函数进行选择？一种有效解决方法就是高斯过程回归(Gaussian process regression，GPR)。

>注：Radford M. Neal，1956年生，加拿大科学家。多伦多大学博士（1995）和教授。贝叶斯神经网络的发明人。导师为Geoffrey Hinton。   
>个人主页：http://www.cs.toronto.edu/~radford/

>Danie G. Krige，1919～2013，南非矿业工程师和统计学家，威特沃特斯兰德大学教授。地理统计学早期的代表人物之一。

http://www.cnblogs.com/hxsyl/p/5229746.html

浅谈高斯过程回归

https://mqshen.gitbooks.io/prml/content/Chapter6/gaussian/gaussian_processes_regression.html

Gaussian Processes Regression

http://www.gaussianprocess.org/gpml/chapters/RW.pdf

Gaussian Processes for Machine Learning

http://people.cs.umass.edu/~wallach/talks/gp_intro.pdf

Introduction to Gaussian Process Regression

http://wenku.baidu.com/view/72f80113915f804d2b16c173.html

高斯过程回归方法综述

https://mp.weixin.qq.com/s/ZE_Chzlgy_7wl9E9q1ckOA

AlphaGo Zero用它来调参？“高斯过程”到底有何过人之处？

https://mp.weixin.qq.com/s/VzN02XW3yN2peqTD6q-4Cg

从数学到实现，全面回顾高斯过程中的函数最优化

https://zhuanlan.zhihu.com/gpml2016

高斯世界下的Machine Learning

https://mp.weixin.qq.com/s/FJAgpbBgRA2Zk3BuiEwjdw

看得见的高斯过程：这是一份直观的入门解读

https://zhuanlan.zhihu.com/p/58987388

多元高斯分布完全解析

https://zhuanlan.zhihu.com/p/73832253

通俗理解高斯过程及其应用

https://zhuanlan.zhihu.com/p/75072046

Gaussian process classification初介绍——回归与分类点点滴滴

# Optimizer进阶

https://mp.weixin.qq.com/s/GS3TvS9nZw-CSJds-Aw_ug

UIUC孙若愚：60页论文综述深度学习优化

https://mp.weixin.qq.com/s/7E8o1TnvmAvZgB7_AWCunQ

2018值得尝试的无参数全局优化新算法

https://mp.weixin.qq.com/s/T-v9OTcJa5OQ71QmYrFtbg

斯坦福大学提出SGD动量自调节器YellowFin

https://mp.weixin.qq.com/s/6u5W7Lm81Wtczdzp5WCJWw

DeepMind提出新型超参数最优化方法：性能超越手动调参和贝叶斯优化

https://mp.weixin.qq.com/s/0V8B-u5_bRM5Fu9oOAYjqw

清华大学：通过在单纯形上软门限投影的加速随机贪心坐标下降

https://mp.weixin.qq.com/s/LuuvvL9yZ3ucXxRq0pZfsg

优化策略：Label Smoothing Regularization_LSR原理分析

https://zhuanlan.zhihu.com/p/23866364

从梯度下降到Hessian-Free优化

https://mp.weixin.qq.com/s/HPrjEdszBSvVoVS66W-Fjw

2017年深度学习优化算法研究亮点最新综述

https://mp.weixin.qq.com/s/W06YcuGWalDbyUaZa_kZnQ

2017年深度学习优化算法最新综述

https://mp.weixin.qq.com/s/WQ6CxRS-v_y-7PnYY-1ffg

在局部误差边界条件下的随机子梯度方法的加速

https://mp.weixin.qq.com/s/rOltA6fDzWmxcSyoHYqeSg

谷歌提出最新参数优化方法Adafactor，已在TensorFlow中开源

https://mp.weixin.qq.com/s/eTVPLSpZir4A49bhmWAibQ

深度学习中的各种优化算法

https://mp.weixin.qq.com/s/CQpIhVinDPhXpp70WhyYww

当前训练神经网络最快的方式：AdamW优化算法+超级收敛

https://mp.weixin.qq.com/s/y3ThoC2A04q4uWiOsuhUJw

腾讯AI Lab提出误差补偿式量化SGD：显著降低分布式机器学习的通信成本

https://mp.weixin.qq.com/s/aBt0qXPHFvwSs-0MtJYKjQ

一文告诉你Adam、AdamW、Amsgrad区别和联系，助你实现Super-convergence的终极目标

https://mp.weixin.qq.com/s/aZlJNZsSv60ZZi2heGo_Mw

一文简述深度学习优化方法——梯度下降

https://mp.weixin.qq.com/s/DsmjjfInV_yPFWB2oSq-dA

取代学习率衰减的新方法：谷歌大脑提出增加Batch Size

https://mp.weixin.qq.com/s/jgOQGDqDKtbJXbAj3EpI9A

别用大批量mini-batch训练神经网络，用局部SGD！

https://zhuanlan.zhihu.com/p/45298186

Matrix Factorization方法证明总结

https://mp.weixin.qq.com/s/0z4mt8iVk5gLRDwbhznV2g

如何理解深度学习的优化？通过分析梯度下降的轨迹

https://mp.weixin.qq.com/s/5MI1J16sEkr4UR4rSrw1wA

Michael Jordan新研究：采样可以比优化更快地收敛

https://mp.weixin.qq.com/s/g8GLF0rf3IPAjRb9wZaS4w

神经网络的奥秘之优化器的妙用

https://mp.weixin.qq.com/s/i-fE4aISTJ0584aIHJ8R0Q

二阶优化！训练ImageNet仅需35个Epoch

https://mp.weixin.qq.com/s/LY1-F5hEyM40DrvobYRexA

腾讯AI Lab&北大提出基于随机路径积分的差分估计子非凸优化方法

https://mp.weixin.qq.com/s/5KyODpSjkdYJ9q-itQDsAA

自Adam出现以来，深度学习优化器发生了什么变化？

https://mp.weixin.qq.com/s/3FSZOlA2sGQwiPj77ShTIQ

最优化算法鸟视解读

https://mp.weixin.qq.com/s/4hSar7SuCjLkZUjuIfu1Lg

如何选择最适合你的学习率变更策略

https://zhuanlan.zhihu.com/p/32923584

Tensorflow中learning rate decay的奇技淫巧

https://mp.weixin.qq.com/s/qk3cw05ZdlYEKDGRG0fnLg

距离几何优化问题--从美国计算机教授追回被抢车辆谈起

https://mp.weixin.qq.com/s/rHkfb1pZhtzVjzYiTRB4WA

交替方向乘子法（ADMM）的基本原理

https://mp.weixin.qq.com/s/4uaaeZSXavbVuU8d1AZA6Q

浅谈交替方向乘子法(ADMM)的经典使用

# 热传导推荐算法

https://www.zhihu.com/question/20184666

推荐系统中用到的热传导算法和物质扩散是怎么用的？

http://tis.hrbeu.edu.cn/oa/pdfdow.aspx?Sid=20160307

基于影响力控制的热传导算法

http://www.doc88.com/p-7082821463697.html

改进的热传导和物质扩散混合推荐算法

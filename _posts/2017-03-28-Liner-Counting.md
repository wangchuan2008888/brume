tags:[ linear counting, big data]

本文参考http://blog.codinglabs.org/articles/algorithms-for-cardinality-estimation-part-ii.html
[TOC]
## 简介
Linear Counting（以下简称LC）在1990年的一篇论文“A linear-time probabilistic counting algorithm for database applications”中被提出。作为一个早期的基数估计算法，LC在空间复杂度方面并不算优秀，实际上LC的空间复杂度与上文中简单bitmap方法是一样的（但是有个常数项级别的降低），都是$O(N_{max})$因此目前很少单独使用LC。不过作为Adaptive Counting等算法的基础，研究一下LC还是比较有价值的。
## 基本算法
### 思路
LC的基本思路是：设有一哈希函数H，其哈希结果空间有m个值（最小值0，最大值m-1），并且哈希结果服从均匀分布。使用一个长度为m的bitmap，每个bit为一个桶，均初始化为0，设一个集合的基数为n，此集合所有元素通过H哈希到bitmap中，如果某一个元素被哈希到第k个比特并且第k个比特为0，则将其置为1。当集合所有元素哈希完成后，设bitmap中还有u个bit为0。则：
$$\widehat{n} = -m \log{\frac{u}{m}}$$
为n的一个估计，且位最大似然估计（MLE）。
示意图如下：

![Linear Counting](/public/images/1.png)

#Bloom Filter简单理解  
---
这两天在看一个分布式Top-k算法——“KLEE”的论文[KLEE - A Framework for Distributed Top-k Query Algorithms](https://ci.apache.org/projects/flink/flink-docs-release-1.0/quickstart/setup_quickstart.html)，其中提及了Bloom Filter的概念，在读了相关部分两遍之后仍然对它的原理不是很明白。于是就google了相关博客，有了一点自己的理解，也不知道是否正确，暂时记录下来，怕日后忘记。
Bloom Filter其实是利用位数组来简洁地表示一个集合，并能判断一个元素是否属于一个集合。这种数据结构或者说算法非常的高效，但是换来的是无法做到0错误，也就是说，当判断一个元素是否属于某个集合的时候，有可能会把不属于这个集合的元素判断为属于该集合，即false positive。所以，Bloom Filter无法适用于那些对错误0容忍的情况。
Bloom Filter用极少的错误换取了存储空间的极大节省。
---
具体是如何实现的呢？
---
##Bloom Filter的算法
---
- 创建一个m位的bitset，将其所有位都初始化位0。
- 选择k个不同的哈希函数。第i个哈希函数对元素e的哈希结果记位hi(e).且值的范围是0到m-1。
- 对于元素e，分别计算h1(e),h2(e),...,hk(e)。然后将bitset的第h1(e),h2(e),...,hk(e)位设为1.
这样就将元素e映射到bitset中的k个二进制位了。
---
那如何检查某个元素是否属于某个集合呢？
---
##Bloom Filter检验  
---
- 对于某个特定集合S，对其中所有元素应用BF算法，得到一个完整bitset(S)。
- 此时若要检验一个元素f是否属于该集合S，则计算h1(f),h2(f),...hk(f)。然后检查bitset(S)的第h1(f),h2(f),...,hk(f)位是否为1，若有任何一位不为1，则可以判定f不属于集合S。若全部位都是1，则“认为”元素f属于集合S。
当然，这种“认为”就是刚才提及的false positive，即使对应位都是1，实际上是无法100%确定该元素属于S的。
---
如何删除已经加入的元素呢？
---
##Bloom Filter元素删除
---
元素加入后会影响其他元素，所以无法删除。实在需要删除元素可以使用Counting Bloom Filter(CBF)，这是一种基本Bloom Filter的变体，CBF将基本的BLoom Filter每一个Bit改为一个计数器，这样就可以删除元素了。
---
##Bloom Filter Parameters
---
- Hash Function选择：一个好的哈希函数要能近似等概率的将字符串映射到各个bit。如果要选择k个hash function可以选择一种函数，然后提供k个不同参数。
- bitset数组大小选择：[Bloom Filters - the math](http://pages.cs.wisc.edu/~cao/papers/summary-cache/node8.html)证明了对于给定的m，n(此处n为集合中元素个数)，当k=ln(2)＊m/n时出错的概率最小。同时该文献还给出特定的k，m，n的出错概率，当k＝10，m＝20＊n时，fals positive发生的概率是0.0000889。


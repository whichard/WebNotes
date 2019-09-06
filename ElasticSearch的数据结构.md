  本文总结了ElasticSearch中用于性能优化所用到的几种数据结构，如用于压缩倒排索引内存存储空间的FST，用于查询条件合并的SkipList以及用于提高范围查找效率的BKDTree，对这几种数据结构在Lucene中的使用进行了详细分析。
### 倒排索引（Inverted Index）存储

很多数据结构均能完成字典功能，总结如下。

| 数据结构                       | 优缺点                                                       |
| ------------------------------ | ------------------------------------------------------------ |
| 排序列表Array/List             | 使用二分法查找，不平衡                                       |
| HashMap/TreeMap                | 性能高，内存消耗大，几乎是原始数据的三倍                     |
| Skip List                      | 跳跃表，可快速查找词语，在lucene、redis、Hbase等均有实现。相对于TreeMap等结构，特别适合高并发场景（[Skip List介绍](http://kenby.iteye.com/blog/1187303)） |
| Trie                           | 适合英文词典，如果系统中存在大量字符串且这些字符串基本没有公共前缀，则相应的trie树将非常消耗内存（[数据结构之trie树](http://dongxicheng.org/structure/trietree/)） |
| Double Array Trie              | 适合做中文词典，内存占用小，很多分词工具均采用此种算法（[深入双数组Trie](http://blog.csdn.net/zhoubl668/article/details/6957830)） |
| Ternary Search Tree            | 三叉树，每一个node有3个节点，兼具省空间和查询快的优点（[Ternary Search Tree](http://www.drdobbs.com/database/ternary-search-trees/184410528)） |
| Finite State Transducers (FST) | 一种有限状态转移机，Lucene 4有开源实现，并大量使用           |

 FST在单个term查询上可能相比hashmap并没有明显优势，甚至会慢一些。但是在范围，前缀搜索以及压缩率上都有明显的优势。

FST = Trie + 有限状态机

下面来看一下由“october”，“november”,”december”构成的FSA.
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190603160442212.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3doaWNoYXJk,size_16,color_FFFFFF,t_70)

对比：Trie只公用前缀，FST公用前缀和后缀。进一步将有限的内存空间进行压缩。

FST的插入删除是较为复杂的。[源码解析](https://www.shenyanchao.cn/blog/2018/12/04/lucene-fst/)

## 查询

### 查询条件合并：使用数据结构SkipList

和传统数据库的索引相比，lucene合并过程中的优化减少了读取数据的IO，倒排合并的灵活性也解决了传统索引较难支持多条件查询的问题。

例如我们需要按名字或者年龄单独查询，也需要进行组合 name = "Alice" and age = "18"的查询，那么使用传统二级索引方案，你可能需要建立两张索引表，然后分别查询结果后进行合并，这样如果age = 18的结果过多的话，查询合并会很耗时。那么在lucene这两个倒排链是怎么合并呢。

假如我们有下面三个倒排链需要进行合并。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190603160551601.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3doaWNoYXJk,size_16,color_FFFFFF,t_70)
在lucene中会采用下列顺序进行合并：

1. 在termA开始遍历，得到第一个元素docId=1
2. Set currentDocId=1
3. 在termB中 search(currentDocId) = 1 (返回大于等于currentDocId的一个doc),
   1. 因为currentDocId ==1，继续
   2. 如果currentDocId 和返回的不相等，执行2，然后继续
4. 到termC后依然符合，返回结果
5. currentDocId = termC的nextItem
6. 然后继续步骤3 依次循环。直到某个倒排链到末尾。

### 范围查找：BKDTree
在lucene中如果想做范围查找，根据上面的FST模型可以看出来，需要遍历FST找到包含这个range的一个点然后进入对应的倒排链，然后进行求并集操作。但是如果是数值类型，比如是浮点数，那么潜在的term可能会非常多，这样查询起来效率会很低。所以为了支持高效的数值类或者多维度查询，lucene引入类BKDTree。BKDTree是基于KDTree，对数据进行按照维度划分建立一棵二叉树确保树两边节点数目平衡。在一维的场景下，KDTree就会退化成一个二叉搜索树，在二叉搜索树中如果我们想查找一个区间，logN的复杂度就会访问到叶子结点得到对应的倒排链。
如果是多维，kdtree的建立流程会发生一些变化。
比如我们以二维为例，建立过程如下：

1. 确定切分维度，这里维度的选取顺序是数据在这个维度方法最大的维度优先。一个直接的理解就是，数据分散越开的维度，我们优先切分。
2. 切分点的选这个维度最中间的点。 
3. 递归进行步骤1，2，我们可以设置一个阈值，点的数目少于多少后就不再切分，直到所有的点都切分好停止。

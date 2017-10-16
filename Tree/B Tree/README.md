## B Tree(Balance Tree)

B树 就是 B-树，中间的横线并不是减号。

如果数据量太多，内存装不下，意味着必须把数据结构放到磁盘上，那么如果减少磁盘 I/O 访问次数，引入 B树 概念。

B树(B-树) 是一种多路平衡查找树，它的每一个节点最多包含 k 个孩子，k 被称为 B树 的阶。k 的大小取决于磁盘页的大小。

一个 m 阶的 B树 具有如下几个特征：

* 根节点至少有两个孩子
* 每个中间节点都包含 k-1 个元素和 k 个孩子，其中 m/2 <= k <= m
* 每一个叶子节点都包含 k-1 个元素，其中 m/2 <= k <= m
* 所有的叶子节点都位于同一层
* 每个节点中的元素从小到大排列，节点当中 k-1 元素正好是 k 个孩子包含的元素的值域划分

磁盘 I/O 访问次数是由树的高度决定。

只要树的高度足够低，磁盘 I/O 访问次数足够少，就可以提升查找性能。

B树 的实际应用：主要应用于文件系统以及部分数据库索引，比如非关系数据库 MongoDB。

## B+ Tree

MySQL 数据库索引就是用 B+ 树结构存储。

B+ 树是基于 B- 树的一种变体，有着比 B- 树更高的查询性能。

一个 m 阶的 B+树 具有如下几个特征：

* 把 k 个子树的中间节点包含有 k 个元素，每个元素不保存数据，只用来索引，所有数据都保存在叶子节点
* 所有的叶子节点中包含了全部元素的信息，及指向含这些元素记录的指针，且叶子节点本身依关键字的大小 自小而大 有序连接
* 所有的中间节点元素都同时存在于子节点，在子节点元素中是最大(或最小)元素。

B+树 每一个父节点的元素都出现在子节点中，是子节点的最大(最小)元素。无论插入删除元素，始终要保持最大元素在根节点当中。
所有叶子节点包含了全部元素的信息，并且每一个叶子节点都带有指向下一个节点的指针，形成了一个有序链表。

B+树 还有一个显著的特点，「卫星数据」的位置，只有叶子节点带有卫星数据，其余中间节点仅仅是索引，没有任何数据关联。

「卫星数据」指的是索引元素所指向的数据记录。比如数据库中的某一行。在 B树 中，无论中间节点还是叶子节点都带有卫星数据。

B+树 的好处主要体现在查询性能上。因为其中间节点没有卫星数据，所以同样大小的磁盘页可以容纳更多的节点元素。

数据量相同的情况下，B+树 的结构比 B树 更加 “矮胖”，因此查询时 I/O 次数也更少。

B+树的查询必须最终查找到叶子节点，而 B树 只要找到匹配元素即可，无论匹配元素处于中间点还是叶子节点。

因此，B树 的查找性能并不稳定(最好情况是查到根节点，最坏情况是查到叶子节点)，而 B+树 的每一次查找都是稳定的。

B树 的范围查询依靠中序遍历，而 B+树 的范围查询只需要在链表上做遍历即可。

## B Tree 与 B+ Tree 对比

B+树 比 B树 的优势在于：

* 单一节点存储更多的元素，使得查询 I/O 次数更少
* 所有查询都要查找到叶子节点，查询性能稳定
* 所有叶子节点形成有序链表，便于范围查询

B树 的好处：虽然查询性能不稳定，但平均的查询速度快一些，不用每次都查找到叶子节点为止。
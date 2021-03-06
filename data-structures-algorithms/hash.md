Hash表 && HashMap
===
### 基本概念
在线性表、树等数据结构中，记录在结构中的相对位置是随机的，和记录的关键字之间不存在确定的关系。因此，当在结构中查找记录时必须进行一系列和关键字的比较。查找记录的效率取决于比较次数。理想的情况是给定一个关键字，不需要进行比较，立刻就能确定该记录在结构中的存储位置，然后一次存取就能得到所需的记录。那么就需要在记录的关键字和记录的存储位置之间建立一个对应关系f，使每个关键字和唯一的一个存储位置相对应。这个对应关系f就称为哈希函数。

哈希函数是从关键字集合到地址集合的映像，值域为哈希表表长允许范围。理想情况下，函数是一对一的，即一个关键字对应唯一的存储位置，反过来，一个存储位置对应唯一的关键字。实际中，可能出现多对一的情况，即不同的关键字对应相同的存储位置，这种现象称为冲突。具有相同函数值的关键字称为同义词。
一般地，冲突只能尽可能地少，不能完全避免。建造哈希表时，必须要有处理冲突的方法。

综上，可如下描述哈希表：
根据设定的哈希函数H(key)和处理冲突的方法将一组关键字映像到一个有限的连续的地址集(区间)上，并以关键字在地址集中的“象”作为记录在表中的存储位置，这种表便称为哈希表，这一映象过程称为哈希造表或散列，所得的存储位置称哈希地址或散列地址。

### 哈希函数的构造方法
能够使冲突尽可能少的哈希函数为好的哈希函数，即关键字映射到地址集中每个地址的概率相等。
#### 直接定址法
取关键字或关键字的某个线性函数值为哈希地址：
H(key) = key 或 H(key) = a * key + b (其中a和b为常数)
假如哈希表中需要存放1949~2014年的出生人口调查表。则可取H(key) = key -1949。如果要查1990年出生的人数，只需查表中第H(key) = 41项即可。
#### 数字分析法
如果哈希表中可能出现的关键字都是事先知道的，则可取关键字中的若干数位组成哈希地址。
假如有80个记录，关键字为8位十进制数，表长为100。则可取关键字中的两位数组成哈希地址。原则是尽量避免产生冲突。
#### 平方取中法
取关键字平方后的中间几位为哈希地址。较常用。
假如想使用数字分析法时，不知道关键字的全部情况，而且选其中哪几位也不一定合适，而一个数平方后的中间几位数和数的每一位都相关，因此平方取中法可使哈希地址随机，冲突减少。取的位数由表长决定。
#### 折叠法
将关键字分割成位数相同的几部分，取这几部分的叠加和(舍去进位)作为哈希地址。关键字位数较多，且每位数字分布均匀时，可使用此法。
数位叠加有两种方法：移位叠加和间界叠加。移位叠加，将每部分的最低位对齐；间界叠加，沿分隔界对折，然后对齐叠加。
例：关键字为0442205864，表长为10000。
移位叠加：5864+4220+04 = 0088
间界叠加：5864+0224+04 = 6092
#### 除留余数法
取关键字被数p除后的余数为哈希地址。p不能大于哈希表表长m。最简单，最常用。可直接对关键字求余，也可在进行折叠、平方取中等运算后求余。
H(key) = key % p , p<=m
一般地，选p为质数或不包含小于20的质因数的合数。
#### 随机数法
选择一个随机函数，取关键字的随机函数值为哈希地址。当关键字长度不等时，使用此法。
H(key) = random(key), random 为随机函数。

### 处理冲突的方法
#### 开放定址法
Hi = (H(key) + di) MOD m, i=1,2,…,k(k<=m-1)，
其中H(key)为哈希函数，m为散列表长，di为增量序列，可有下列三种取法：
di=1,2,3,…, m-1,称线性探测再散列；
di=12, -12, 22,-22,32, …, ±k2,(k<=m/2)称二次探测再散列；
di=伪随机数序列，称伪随机探测再散列。
两个记录的第一个哈希地址不同，但是在处理冲突的过程中，争夺同一个后继哈希地址的现象称为二次聚集。即处理同义词的冲突过程中又出现了非同义词的冲突。
#### 再哈希法
Hi = RHi(key), i=1,2,…,k
RHi均是不同的散列函数，即在同义词产生地址冲突时计算另一个散列函数地址，直到冲突不再发生，这种方法不易产生“聚集”，但增加了计算时间。
#### 链地址法
将所有关键字为同义词的记录存储在同一线性链表中。假设某哈希函数产生的哈希地址在区间[0,m-1]上，则设立一个指针型向量：
Chain ChainHash[m]
其每个分量的初始状态都是空指针。凡哈希地址为i的记录都插入到头指针为ChainHash[i]的链表中。在链表中的插入位置可以在表头或表尾，也可以在中间，以保持同义词在同一线性链表中按关键字有序。
#### 建立一个公共溢出区
基本表HashTable[]中每个位置存放一个记录，将和基本表中发生冲突的记录全部存放到溢出表OverTable[]中。

### 哈希表的查找和分析
查找过程：给定key值，根据哈希函数求得哈希地址，若表中此位置上没有记录，则查找不成功；若有记录，则比较关键字，若和key值相同，则查找成功；若不同，则根据处理冲突的方法找“下一地址”，直至哈希表中某个位置为“空”或者某记录的关键字等于key值时为止。
**衡量哈希表的效率：**平均查找长度。取决于哈希函数，处理冲突的方法和装填因子(表中的记录数/表长)。
**注意：若处理冲突的方法不是链地址法，则在哈希表中删除一个记录时，需要在该位置插入一个特殊符号，以免找不到在它之后填入的同义词记录。**

---

## HashMap的工作原理

HashMap基于hashing原理，我们通过put()和get()方法储存和获取对象。当我们将键值对传递给put()方法时，它调用键对象的hashCode()方法来计算hashcode，让后找到bucket位置来储存值对象。当获取对象时，通过键对象的equals()方法找到正确的键值对，然后返回值对象。HashMap使用链表来解决碰撞问题，当发生碰撞了，对象将会储存在链表的下一个节点中。 HashMap在每个链表节点中储存键值对对象。

当两个不同的键对象的hashcode相同时会发生什么？ 它们会储存在同一个bucket位置的链表中。键对象的equals()方法用来找到键值对。

因为HashMap的好处非常多，我曾经在电子商务的应用中使用HashMap作为缓存。因为金融领域非常多的运用Java，也出于性能的考虑，我们会经常用到HashMap和ConcurrentHashMap。


**为什么String, Interger这样的wrapper类适合作为键？** 
String, Interger这样的wrapper类作为HashMap的键是再适合不过了，而且String最为常用。因为String是不可变的，也是final的，而且已经重写了equals()和hashCode()方法了。其他的wrapper类也有这个特点。不可变性是必要的，因为为了要计算hashCode()，就要防止键值改变，如果键值在放入时和获取时返回不同的hashcode的话，那么就不能从HashMap中找到你想要的对象。不可变性还有其他的优点如线程安全。如果你可以仅仅通过将某个field声明成final就能保证hashCode是不变的，那么请这么做吧。因为获取对象的时候要用到equals()和hashCode()方法，那么键对象正确的重写这两个方法是非常重要的。如果两个不相等的对象返回不同的hashcode的话，那么碰撞的几率就会小些，这样就能提高HashMap的性能。

**我们可以使用自定义的对象作为键吗？** 这是前一个问题的延伸。当然你可能使用任何对象作为键，只要它遵守了equals()和hashCode()方法的定义规则，并且当对象插入到Map中之后将不会再改变了。如果这个自定义对象时不可变的，那么它已经满足了作为键的条件，因为当它创建之后就已经不能改变了。

**我们可以使用CocurrentHashMap来代替Hashtable吗？**
我们知道Hashtable是synchronized的，但是ConcurrentHashMap同步性能更好，因为它仅仅根据同步级别对map的一部分进行上锁。ConcurrentHashMap当然可以代替HashTable，但是HashTable提供更强的线程安全性。看看这篇博客查看Hashtable和ConcurrentHashMap的区别。


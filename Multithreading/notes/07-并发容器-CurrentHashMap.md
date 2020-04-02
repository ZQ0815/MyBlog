# Hashtable
## Hashtable与HashMap的区别
- Hashtable继承自Dictionary类，而HashMap继承自AbstractMap类。但二者都实现了Map接口。
- Hashtable中的所有方法都用synchronized关键字修饰，使用的时候不需要自己去同步。
- Key和value都不允许出现Null值
- HashTable直接使用对象的hashCode而HashMap重新计算hash值。
- HashTable中hash数组默认大小是11，增加的方式是 old*2+1。
- 还多了Contains等方法。
>由于HashTable在每个方法中都使用synchronized，因此在高并发的情况下其性能仍然比较差。

# ConcurrentHashMap

## 内存结构
- JDK1.8之前
![ConcurrentHashMap存储结构](../../img/ConcurrentHashMap存储结构.png)
- JDK1.8之后
![ConcurrentHashMap存储结构-18](../../img/ConcurrentHashMap存储结构-18.png)

## 性能提升
- JDK1.8之前
ConcurrentHashMap使用的是分段锁技术,将ConcurrentHashMap将锁一段一段的存储，然后给每一段数据配一把锁（segment），当一个线程占用一把锁（segment）访问其中一段数据的时候，其他段的数据也能被其它的线程访问，默认分配16个segment。默认比Hashtable效率提高16倍。

- JDK1.8之后
ConcurrentHashMap取消了segment分段锁，而采用CAS和synchronized来保证并发安全。数据结构跟HashMap1.8的结构一样，数组+链表/红黑二叉树。
synchronized只锁定当前链表或红黑二叉树的首节点，这样只要hash不冲突，就不会产生并发，效率又提升N倍。

## JDK1.8之前

### Segment结构
```
static final class Segment<K,V> extends ReentrantLock implements Serializable {
    ...

    transient volatile HashEntry<K,V>[] table;
    // threshold阈，是哈希表在其容量自动增加之前可以达到多满的一种尺度。
    transient int threshold;
    // loadFactor是加载因子
    final float loadFactor;

    Segment(float lf, int threshold, HashEntry<K,V>[] tab) {
        this.loadFactor = lf;
        this.threshold = threshold;
        this.table = tab;
    }

    ...
}
```
1. 首先Segment继承了ReentrantLock，只有当线程竞争同一个Segment的时候才会对阻塞，并发效率比HashTable提升了很多倍。
2. 其次Segment获取锁有一个自旋的操作，能提升并发效率。

### put操作
### hash算法
### 定位索引
### 扩容机制
### 加锁过程
>[参考文献](https://www.cnblogs.com/skywang12345/p/3498537.html)
## JDK1.8之后
>[参考文献](https://blog.csdn.net/programmer_at/article/details/79715177)

### 数据结构
```
class Node<K,V> implements Map.Entry<K,V> {
    final int hash;
    final K key;
    volatile V val;
    volatile Node<K,V> next;
    //... 省略部分代码
}
```
### put操作
假设table已经初始化完成，put操作采用==CAS+synchronized==实现并发插入或更新操作：
- 当前bucket为空时，使用CAS操作，将Node放入对应的bucket中。
- 出现hash冲突，则采用synchronized关键字。倘若当前hash对应的节点是链表的头节点，遍历链表，若找到对应的node节点，则修改node节点的val，否则在链表末尾添加node节点；倘若当前节点是红黑树的根节点，在树结构上遍历元素，更新或增加节点。
- 倘若当前map正在扩容f.hash == MOVED， 则跟其他线程一起进行扩容

### hash算法
```
static final int HASH_BITS = 0x7fffffff; // usable bits of normal node hash
static final int spread(int h) {return (h ^ (h >>> 16)) & HASH_BITS;}
```
### 定位索引
```
int index = (n - 1) & hash  // n为bucket的个数
```
### 扩容机制
什么时候会触发扩容？
- 如果新增节点之后，所在的链表的元素个数大于等于8，则会调用treeifyBin把链表转换为红黑树。在转换结构时，若tab的长度小于MIN_TREEIFY_CAPACITY，默认值为64，则会将数组长度扩大到原来的两倍，并触发transfer，重新调整节点位置。（只有当tab.length >= 64, ConcurrentHashMap才会使用红黑树。）
- 新增节点后，addCount统计tab中的节点个数大于阈值（sizeCtl），会触发transfer，重新调整节点位置。

1. transfer

当table的元素数量达到容量阈值sizeCtl，需要对table进行扩容：
- 构建一个nextTable，大小为table两倍
- 把table的数据复制到nextTable中。
在扩容过程中，依然支持并发更新操作；也支持并发插入。
```
private final void transfer(Node<K,V>[] tab, Node<K,V>[] nextTab)
```
如何在扩容时，并发地复制与插入？
1. 遍历整个table，当前节点为空，则采用CAS的方式在当前位置放入fwd
2. 当前节点已经为fwd(with hash field “MOVED”)，则已经有有线程处理完了了，直接跳过 ，这里是控制并发扩容的核心
3. 当前节点为链表节点或红黑树，重新计算链表节点的hash值，移动到nextTable相应的位置（构建了一个反序链表和顺序链表，分别放置在i和i+n的位置上）。移动完成后，用Unsafe.putObjectVolatile在tab的原位置赋为为fwd, 表示当前节点已经完成扩容。

>此处遗留一个问题：红黑树在扩容时是如何分别构建正序与反序链表的？
### 加锁过程
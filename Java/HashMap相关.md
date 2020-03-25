<!-- TOC -->

- [HashMap相关面试问题](#hashmap相关面试问题)
    - [你知道HashMap的工作原理吗？你知道HashMap的get()方法的工作原理吗？](#你知道hashmap的工作原理吗你知道hashmap的get方法的工作原理吗)
    - [当两个对象的hashcode相同会发生什么？](#当两个对象的hashcode相同会发生什么)
    - [如果两个键的hashcode相同，你如何获取值对象？](#如果两个键的hashcode相同你如何获取值对象)
    - [如果HashMap的大小超过了负载因子(load factor)定义的容量，怎么办？](#如果hashmap的大小超过了负载因子load-factor定义的容量怎么办)
    - [你了解重新调整HashMap大小存在什么问题吗？](#你了解重新调整hashmap大小存在什么问题吗)
    - [你说HashMap的get迭代了一个链表，那怎么保证HashMap的时间复杂度O(1)?链表的查找的时间复杂度又是多少？](#你说hashmap的get迭代了一个链表那怎么保证hashmap的时间复杂度o1链表的查找的时间复杂度又是多少)
    - [put时，是加到链表头还是链表尾](#put时是加到链表头还是链表尾)
    - [传统 HashMap 的缺点](#传统-hashmap-的缺点)
    - [HashMap Hashtable LinkedHashMap 和TreeMap](#hashmap-hashtable-linkedhashmap-和treemap)

<!-- /TOC -->
#  HashMap相关面试问题

##  你知道HashMap的工作原理吗？你知道HashMap的get()方法的工作原理吗？

HashMap是基于hashing的原理，我们使用put(key, value)存储对象到HashMap中，使用get(key)从HashMap中获取对象。当我们给put()方法传递键和值时，我们先对键调用hashCode()方法，返回的hashCode用于找到bucket位置来储存Entry对象。

##  当两个对象的hashcode相同会发生什么？

因为hashcode相同，所以它们的bucket位置相同，‘碰撞’会发生。因为HashMap使用LinkedList存储对象，这个Entry(包含有键值对的Map.Entry对象)会存储在LinkedList中。(当向 HashMap 中添加 key-value 对，由其 key 的 hashCode() 返回值决定该 key-value 对（就是 Entry 对象）的存储位置。当两个 Entry 对象的 key 的 hashCode() 返回值相同时，将由 key 通过 eqauls() 比较值决定是采用覆盖行为（返回 true），还是产生 Entry 链（返回 false）。)，此时若你能讲解JDK1.8红黑树引入，面试官或许会刮目相看。

##  如果两个键的hashcode相同，你如何获取值对象？

当我们调用get()方法，HashMap会使用键对象的hashcode找到bucket位置，然后获取值对象。如果有两个值对象储存在同一个bucket，将会遍历LinkedList直到找到值对象。找到bucket位置之后，会调用keys.equals()方法去找到LinkedList中正确的节点，最终找到要找的值对象。(当程序通过 key 取出对应 value 时，系统只要先计算出该 key 的 hashCode() 返回值，在根据该 hashCode 返回值找出该 key 在 table 数组中的索引，然后取出该索引处的 Entry，最后返回该 key 对应的 value 即可。)

##  如果HashMap的大小超过了负载因子(load factor)定义的容量，怎么办？

当一个map填满了75%的bucket时候，和其它集合类(如ArrayList等)一样，将会创建原来HashMap大小的两倍的bucket数组，来重新调整map的大小，并将原来的对象放入新的bucket数组中。这个过程叫作rehashing，因为它调用hash方法找到新的bucket位置。

##  你了解重新调整HashMap大小存在什么问题吗？

当重新调整HashMap大小的时候，确实存在条件竞争，因为如果两个线程都发现HashMap需要重新调整大小了，它们会同时试着调整大小。在调整大小的过程中，存储在LinkedList中的元素的次序会反过来，因为移动到新的bucket位置的时候，HashMap并不会将元素放在LinkedList的尾部，而是放在头部，这是为了避免尾部遍历(tail traversing)。如果条件竞争发生了，那么就死循环了。这个时候，你可以质问面试官，为什么这么奇怪，要在多线程的环境下使用HashMap呢？

##  你说HashMap的get迭代了一个链表，那怎么保证HashMap的时间复杂度O(1)?链表的查找的时间复杂度又是多少？

分四步：

* 判断key，根据key算出索引。

* 根据索引获得索引位置所对应的键值对链表。

* 遍历键值对链表，根据key找到对应的Entry键值对。

* 拿到value。

分析：

以上四步要保证HashMap的时间复杂度O(1)，需要保证每一步都是O(1)，现在看起来就第三步对链表的循环的时间复杂度影响最大，链表查找的时间复杂度为O(n)，与链表长度有关。我们要保证那个链表长度为1，才可以说时间复杂度能满足O(1)。但这么说来只有那个hash算法尽量减少冲突，才能使链表长度尽可能短，理想状态为1。因此可以得出结论：HashMap的查找时间复杂度只有在最理想的情况下才会为O(1)，而要保证这个理想状态不是我们开发者控制的。

##  put时，是加到链表头还是链表尾

jdk8之前的是链表头，JDK8是链表尾

##  传统 HashMap 的缺点

JDK 1.8 以前 HashMap 的实现是 数组+链表，即使哈希函数取得再好，也很难达到元素百分百均匀分布。

当 HashMap 中有大量的元素都存放到同一个桶中时，这个桶下有一条长长的链表，这个时候 HashMap 就相当于一个单链表，假如单链表有 n 个元素，遍历的时间复杂度就是 O(n)，完全失去了它的优势。

针对这种情况，JDK 1.8 中引入了 红黑树（查找时间复杂度为 O(logn)）来优化这个问题。

##  HashMap Hashtable LinkedHashMap 和TreeMap

Map主要用于存储健值对，根据键得到值，因此不允许键重复(重复了覆盖了),但允许值重复。

Hashmap 是一个最常用的Map,它根据键的HashCode值存储数据,根据键可以直接获取它的值，具有很快的访问速度，遍历时，取得数据的顺序是完全随机的。 HashMap最多只允许一条记录的键为Null;允许多条记录的值为 Null;HashMap不支持线程的同步，即任一时刻可以有多个线程同时写HashMap;可能会导致数据的不一致。如果需要同步，可以用 Collections的synchronizedMap方法使HashMap具有同步的能力，或者使用ConcurrentHashMap。

Hashtable与 HashMap类似,它继承自Dictionary类，不同的是:它不允许记录的键或者值为空;它支持线程的同步，即任一时刻只有一个线程能写Hashtable,因此也导致了 Hashtable在写入时会比较慢。

LinkedHashMap 是HashMap的一个子类，保存了记录的插入顺序，在用Iterator遍历LinkedHashMap时，先得到的记录肯定是先插入的.也可以在构造时用带参数，按照应用次数排序。在遍历的时候会比HashMap慢，不过有种情况例外，当HashMap容量很大，实际数据较少时，遍历起来可能会比 LinkedHashMap慢，因为LinkedHashMap的遍历速度只和实际数据有关，和容量无关，而HashMap的遍历速度和他的容量有关。

TreeMap实现SortMap接口，能够把它保存的记录根据键排序,默认是按键值的升序排序，也可以指定排序的比较器，当用Iterator 遍历TreeMap时，得到的记录是排过序的。TreeMap不允许键为NULL，允许值为NULL

一般情况下，我们用的最多的是HashMap,在Map 中插入、删除和定位元素，HashMap 是最好的选择。但如果您要按自然顺序或自定义顺序遍历键，那么TreeMap会更好。如果需要输出的顺序和输入的相同,那么用LinkedHashMap 可以实现,它还可以按读取顺序来排列.

HashMap是一个最常用的Map，它根据键的hashCode值存储数据，根据键可以直接获取它的值，具有很快的访问速度。HashMap最多只允许一条记录的键为NULL，允许多条记录的值为NULL。

HashMap不支持线程同步，即任一时刻可以有多个线程同时写HashMap，可能会导致数据的不一致性。如果需要同步，可以用Collections的synchronizedMap方法使HashMap具有同步的能力。

Hashtable与HashMap类似，不同的是：它不允许记录的键或者值为空；它支持线程的同步，即任一时刻只有一个线程能写Hashtable，因此也导致了Hashtable在写入时会比较慢。

LinkedHashMap保存了记录的插入顺序，在用Iterator遍历LinkedHashMap时，先得到的记录肯定是先插入的。在遍历的时候会比HashMap慢

TreeMap能够把它保存的记录根据键排序，默认是按升序排序，也可以指定排序的比较器。当用Iterator遍历TreeMap时，得到的记录是排过序的。


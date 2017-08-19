title: Java集合类详解
date: 2017-08-07 15:32:43
tags: [java,集合,map,list]
category: 笔记
---
```
Collection
├List
│├LinkedList
│├ArrayList
│└Vector
│　└Stack
└Set
Map
├Hashtable
├HashMap
└WeakHashMap 

```
{% asset_img Collection.png [Collection] %}

{% asset_img map.png [map] %}

## (1)HashMap和Hashtable
a.HashMap不是线程安全的；HashTable是线程安全的，其线程安全是通过Sychronize实现。

b.由于上述原因，HashMap效率高于HashTable。

c.HashMap的键可以为null，HashTable不可以。

d.多线程环境下，通常也不是用HashTable，因为效率低。HashMap配合Collections工具类使用实现线程安全。同时还有ConcurrentHashMap可以选择，该类的线程安全是通过Lock的方式实现的，所以效率高于Hashtable。

数组，链表，哈希表。各有优劣，顺便提一下，数组连续内存空间，查找速度快，增删慢；链表充分利用了内存，存储空间是不连续的，首尾存储上下一个节点的信息，所以寻址麻烦，查找速度慢，但是增删快；哈希表呢，综合了它们两个的有点，一个哈希表，由数组和链表组成。假设一条链表有1000个节点，现在查找最后一个节点，就得从第一个遍历到最后一个；如果用哈希表，将这条链表分为10组，用一个容量为10数组来存储这10组链表的头结点（a[0] = 0 , a[1] = 100 , a[2] = 200 …）。这样寻址就快了。

HashMap实现原理就是上述原理了，当然其具体实现还有很多其他的东西。Hashtable同理，只不过做了同步处理。
Hash碰撞，不同的key根据hash算法算出的值可能一样，如果一样就是所谓的碰撞

优化措施：

(1) HashMap的扩容代价非常大，要生成一个新的桶数组，然后要把所有元素都重新Hash落桶一次，几乎等于重新执行了一次所有元素的put。所以如果我们对Map的大小有一个范围的话，可以在构造时给定大小，一般大小设置为：(int) ((float) expectedSize / 0.75F + 1.0F)。

(2) key的设计尽量简洁。

### HashMap遍历
```
 //遍历方式一:for each遍历HashMap的entryset，注意这种方式在定义的时候就必须写成
//Map<Integer , String> hs，不能写成Map hs;
for(Entry<Integer , String> entry : hs.entrySet()){
    System.out.println("key:"+entry.getKey()+"  value:"+entry.getValue());
}
//遍历方式二：使用EntrySet的Iterator
Iterator<Map.Entry<Integer , String>> iterator = hs.entrySet().iterator();
while(iterator.hasNext()){
    Entry<Integer , String> entry =  iterator.next();
    System.out.println("key:"+entry.getKey()+"  value:"+entry.getValue());
};
//遍历方式三：for each直接使用HashMap的keyset
for(Integer key : hs.keySet()){
    System.out.println("key:"+key+"  value:"+hs.get(key));
};
//遍历方式四：使用keyset的Iterator
Iterator keyIterator = hs.keySet().iterator();
while(keyIterator.hasNext()){
    Integer key = (Integer)keyIterator.next();
    System.out.println("key:"+key+"  value:"+hs.get(key));
} 
```
（1）使用keyset的两种方式都会遍历两次，所以效率没有使用EntrySet高。

（2）HashMap输出是无序的，这个无序不是说每次遍历的结果顺序不一样，而是说与插入顺序不一样

### HashMap线程同步
第一种

```
Map<Integer , String> hs = new HashMap<Integer , String>();
hs = Collections.synchronizedMap(hs);
```

第二种 

```
ConcurrentHashMap<Integer , String> hs = new ConcurrentHashMap<Integer , String>();
```

## ArrayList , LinkedList , Vector
(1)ArrayList和Vector本质都是用数组实现的，而LinkList是用双链表实现的；所以，Arraylist和Vector在查找效率上比较高，增删效率比较低；LinkedList则正好相反。ArrayList是线程不安全的，Vector是线程安全的，效率肯定没有ArrayList高了。实际中一般也不怎么用Vector,可以自己做线程同步，也可以用Collections配合ArrayList实现线程同步。


Map主要用于存储健值对，根据键得到值，因此不允许键重复(重复了覆盖了),但允许值重复。
Hashmap 是一个最常用的Map,它根据键的HashCode 值存储数据,根据键可以直接获取它的值，具有很快的访问速度，遍历时，取得数据的顺序是完全随机的。HashMap最多只允许一条记录的键为Null;允许多条记录的值为 Null;HashMap不支持线程的同步，即任一时刻可以有多个线程同时写HashMap;可能会导致数据的不一致。如果需要同步，可以用 Collections的synchronizedMap方法使HashMap具有同步的能力，或者使用ConcurrentHashMap。
Hashtable与 HashMap类似,它继承自Dictionary类，不同的是:它不允许记录的键或者值为空;它支持线程的同步，即任一时刻只有一个线程能写Hashtable,因此也导致了 Hashtable在写入时会比较慢。
LinkedHashMap保存了记录的插入顺序，在用Iterator遍历LinkedHashMap时，先得到的记录肯定是先插入的.也可以在构造时用带参数，按照应用次数排序。在遍历的时候会比HashMap慢，不过有种情况例外，当HashMap容量很大，实际数据较少时，遍历起来可能会比LinkedHashMap慢，因为LinkedHashMap的遍历速度只和实际数据有关，和容量无关，而HashMap的遍历速度和他的容量有关。
TreeMap实现SortMap接口，能够把它保存的记录根据键排序,默认是按键值的升序排序，也可以指定排序的比较器，当用Iterator 遍历TreeMap时，得到的记录是排过序的。

一般情况下，我们用的最多的是HashMap,HashMap里面存入的键值对在取出的时候是随机的,它根据键的HashCode值存储数据,根据键可以直接获取它的值，具有很快的访问速度。在Map 中插入、删除和定位元素，HashMap 是最好的选择。
TreeMap取出来的是排序后的键值对。但如果您要按自然顺序或自定义顺序遍历键，那么TreeMap会更好。
LinkedHashMap 是HashMap的一个子类，如果需要输出的顺序和输入的相同,那么用LinkedHashMap可以实现,它还可以按读取顺序来排列，像连接池中可以应用。

 

 

1. HashSet是通过HashMap实现的,TreeSet是通过TreeMap实现的,只不过Set用的只是Map的key
2. Map的key和Set都有一个共同的特性就是集合的唯一性.TreeMap更是多了一个排序的功能.
3. hashCode和equal()是HashMap用的, 因为无需排序所以只需要关注定位和唯一性即可.
   a.hashCode是用来计算hash值的,hash值是用来确定hash表索引的.
   b.hash表中的一个索引处存放的是一张链表, 所以还要通过equal方法循环比较链上的每一个对象
       才可以真正定位到键值对应的Entry.
   c.put时,如果hash表中没定位到,就在链表前加一个Entry,如果定位到了,则更换Entry中的value,并返回旧value
4. 由于TreeMap需要排序,所以需要一个Comparator为键值进行大小比较.当然也是用Comparator定位的.
   a.Comparator可以在创建TreeMap时指定
   b.如果创建时没有确定,那么就会使用key.compareTo()方法,这就要求key必须实现Comparable接口.
   c.TreeMap是使用Tree数据结构实现的,所以使用compare接口就可以完成定位了.

 

 

注意： 
1、Collection没有get()方法来取得某个元素。只能通过iterator()遍历元素。 
2、Set和Collection拥有一模一样的接口。 
3、List，可以通过get()方法来一次取出一个元素。使用数字来选择一堆对象中的一个，get(0)...。(add/get) 
4、一般使用ArrayList。用LinkedList构造堆栈stack、队列queue。 
5、Map用 put(k,v) / get(k)，还可以使用containsKey()/containsValue()来检查其中是否含有某个key/value。 
   HashMap会利用对象的hashCode来快速找到key。
   hashing 哈希码就是将对象的信息经过一些转变形成一个独一无二的int值，这个值存储在一个array中。
   我们都知道所有存储结构中，array查找速度是最快的。所以，可以加速查找。
   发生碰撞时，让array指向多个values。即，数组每个位置上又生成一个梿表。
6、Map中元素，可以将key序列、value序列单独抽取出来。 
使用keySet()抽取key序列，将map中的所有keys生成一个Set。 
使用values()抽取value序列，将map中的所有values生成一个Collection。 
为什么一个生成Set，一个生成Collection？那是因为，key总是独一无二的，value允许重复。



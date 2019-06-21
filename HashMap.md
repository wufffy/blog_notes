# 源码学习
### HashMap 中的字段
``` java

    //默认初始容量为16
    static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 
    //map的最大的容量
    static final int MAXIMUM_CAPACITY = 1 << 30;
    //默认加载因子 0.75
    static final float DEFAULT_LOAD_FACTOR = 0.75f;
    //链表长度大于8时 抓换为红黑树 
    static final int TREEIFY_THRESHOLD = 8;
    //删除节点后,链表长度小于6 时 红黑树变化为链表
    static final int UNTREEIFY_THRESHOLD = 6;
    // 扩容的临界值
    static final int MIN_TREEIFY_CAPACITY = 64;
    //存储元素的数组
    transient Node<K,V>[] table;
    //缓存的 entityset 用于 AbstractMap 的  keySet() and values()方法
    transient Set<Map.Entry<K,V>> entrySet;
    //map key-value 元素的个数
    transient int size;
    //map的修改次数,每一次修改都会修改这个值用这个值来保证 快速失败(fail-fast)
    transient int modCount;
    //要扩容的临界值  大小 capacity(容量) * loadfactor(加载因子)
    int threshold;
    //加载因子,没有指定为默认加载因子
    final float loadFactor;
```
### HashMap 中的内部类 Node 链表的节点
```java
// 继承自 Map.Entry<K,V>
static class Node<K,V> implements Map.Entry<K,V> {
       final int hash;
       final K key;
       V value;
       // 下一个节点
       Node<K,V> next;
       Node(int hash, K key, V value, Node<K,V> next) {
            this.hash = hash;
            this.key = key;
            this.value = value;
            this.next = next;
        }
        public final K getKey()        { return key; }
        public final V getValue()      { return value; }
        public final String toString() { return key + "=" + value; }
        // 返回 Hash 值
        public final int hashCode() {
            return Objects.hashCode(key) ^ Objects.hashCode(value);
        }
        //设置新值 返回旧值
        public final V setValue(V newValue) {
            V oldValue = value;
            value = newValue;
            return oldValue;
        }
        // 重写 equals() 
        public final boolean equals(Object o) {
            if (o == this)
                return true;
            if (o instanceof Map.Entry) {
                Map.Entry<?,?> e = (Map.Entry<?,?>)o;
                if (Objects.equals(key, e.getKey()) &&
                    Objects.equals(value, e.getValue()))
                    return true;
            }
            return false;
        }
}
```

### HashMap 中的内部类 TreeNode 红黑树 的节点 里面都是红黑树的方法,太长先不写了

```java
//继承 LinkedHashMap 
static final class TreeNode<K,V> extends LinkedHashMap.Entry<K,V> {
        TreeNode<K,V> parent;  // red-black tree links
        TreeNode<K,V> left;
        TreeNode<K,V> right;
        TreeNode<K,V> prev;    // needed to unlink next upon deletion
        boolean red;
        TreeNode(int hash, K key, V val, Node<K,V> next) {
            super(hash, key, val, next);
        }

        /**
         * Returns root of tree containing this node.
         */
        final TreeNode<K,V> root() {
            for (TreeNode<K,V> r = this, p;;) {
                if ((p = r.parent) == null)
                    return r;
                r = p;
            }
        } 
        ......
```


### 4个构造函数
> 指定初始容量个 加载因子大小的构造函数

```java
 /**
     * Constructs an empty <tt>HashMap</tt> with the specified initial
     * capacity and load factor.
     *
     * @param  initialCapacity 初始容量大小,默认16
     * @param  loadFactor     加载因子,默认0.75f
     * @throws IllegalArgumentException  会进行参数校验,不通过排除异常
     */
    public HashMap(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal initial capacity: " + initialCapacity);
            //初始容量不能大于 最大容量
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;

        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal load factor: " + loadFactor);
            //设置加载因子
        this.loadFactor = loadFactor;
        //获取初始容量最接近的 2次幂
        this.threshold = tableSizeFor(initialCapacity);
    }


```

>  其他构造函数都会调用上面的构造函数

```
 public HashMap(int initialCapacity) {
        this(initialCapacity, DEFAULT_LOAD_FACTOR);
    }

 public HashMap() {
        this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted
    }
//特殊的构造函数
public HashMap(Map<? extends K, ? extends V> m) {
        this.loadFactor = DEFAULT_LOAD_FACTOR;
        putMapEntries(m, false);
    }
```
### 首先要接收一个capacity参数作为缓存的最大容量，然后实现两个API，一个是put(key,val)方法存入键值对，另外一个是get(key)方法获取key队应的val，如果key不存在，则返回-1

```
//缓存容量为2
LRUCasche cache = new LRUCache(2);
//你可以把cache理解为一个队列
//假设左边是队头，右边是队尾
//最近使用的排在队头，久未使用的排在队尾
//圆括号表示键值对(key，val)

cache.put(1,1);
//cache = [(1,1)]

cache.put(2,2);
//cache = [(1,1),(2,2)]

cache.get(1); //返回1
//cache = [(1,1), (2,2)]
//解释：因为最近访问了键1，所以提升到队头
//返回键1对应的值1

cache.put(3,3);
//cache = [(3,3),(1,1)]
//解释：缓存容量满了，需要删除内容空出位置
//优先删除久未使用的数据，队尾数据
//然后新数据插入队头

cache.get(2);   // 返回-1

cache.put(1,4);
//cache = [(1,4),(3,3)]
//解释：键1已存在，把原始值1覆盖为4
```

> 要想让put和get方法的时间复杂度为O(1), cache这个数据结构必备以下条件：
1. cache中的元素必须有时序，以区分最近使用的和久未使用的数据，当容量满了要删除久未使用的那个元素腾挪位置
2. 要在cache中快速获得某个key，是否存在对应的val
3. 每次访问cache中的某个key，需要将这个元素变为最近使用的，也就是说cache要支持在任意位置快速插入和删除元素

```
class Node {
    public int key, val;
    public Node next, prev;

    public Node(int key, int val) {
        this.key = key;
        this.val = val;
    }
}

class DoubleList {
    // 头尾虚节点
    private Node head, tail;
    //链表元素
    private int size;

    public DoubleList() {
        //初始化双向链表
        head = new Node(0, 0);
        tail = new Node(0, 0);
        head.next = tail;
        tail.prev = head;
        size = 0;
    }

    //在链表尾部添加节点x，时间复杂度为O(1)
    public void addLast(Node x) {
        x.prev = tail.prev;
        x.next = tail;
        tail.prev.next = x;
        tail.prev = x;
        size++;
    }

    //删除链表中的x节点，x一定存在
    public void remove(Node x) {
        x.prev.next = x.next;
        x.next.prev = x.prev;
        size--;
    }

    //删除链表中的第一个节点，并返回该节点，时间复杂度为O(1)
    public Node removeFirst() {
        if (head.next == tail) {
            return null;
        }
        Node first = head.next;
        remove(first);
        return first;
    }

    //返回链表长度，时间复杂度为O(1)
    public int size() {
        return size;
    }
}
```
1. 为什么使用双向链表： 我们需要删除操作，所以删除一个节点不仅需要该节点，还需要该节点的前驱节点，保证时间复杂度为O(1)
2. 为什么链表中同时存储key和val，删除的时候，需要获取key从map中删除

```
class LRUCache {
    // key -> Node(key,val)
    private HashMap<Integer, Node> map;
    //Node(k1,v1) <-> Node(k2,v2)
    private DoubleList cache;
    //最大容量
    private int capacity;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        map = new HashMap<>();
        cache = new DoubleList();
    }

    // 由于同时要维护一个map和cache，所以比较容易漏掉一些操作，所以提供一层抽象

    //将某个key提升为最近使用
    private void makeRecently(int key) {
        Node x = map.get(key);
        //先从链表中删除这个节点
        cache.remove(x);
        //重新插入到队尾
        cache.addLast(x);
    }

    //添加最近使用的元素
    private void addRecently(int key, int val) {
        Node x = new Node(key, val);
        //链表尾部就是最近使用的元素
        cache.addLast(x);
        //在map中添加key的映射
        map.put(key, x);
    }

    //删除一个key
    private void deleteKey(int key) {
        Node x = map.get(key);
        //从链表中删除
        cache.remove(x);
        //从map中删除
        map.remove(key);

    }

    //删除最久未使用的元素
    private void removeLeastRecently() {
        //链表头部的第一个元素就是最久未使用的
        Node deletedNode = cache.removeFirst();
        //同时从map中删除
        int deletedKey = deletedNode.key;
        map.remove(deletedKey);
    }
}

```

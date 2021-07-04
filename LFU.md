class LFUCache {
    // 调用get的时候，返回该key的val
    // 只要用get或者put访问一次某个key，该key的freq就要+1
    // 如果容量满了的时候进行插入，就将freq最小的key删除， 如果最小的fre对应多个key，删除最旧的


    //1. 使用一个hashMap存储key和val
    //2. 使用一个hashMap存储key和freq
    //3.1 需要freq到key的映射
    //3.2 将freq最小的key删除，快速得到，需要时间复杂度事O(1)，那就不能遍历，需要一个变量minFreq来记录当前最小的freq
    //3/3 可能有多个key拥有相同的freq，所以freq对key是一对多关系，即一个freq对应一个key的列表
    //3.4 希望freq对应的key的列表是存在时序的。这样便于快速查找并删除最旧的key
    //3.5 希望能快速删除key列表中的任何一个key，因为如果频次为freq的某个key被访问，那么频次就会变成freq+1，就应该从freq的key列表中给删除，添加到freq+1的对应的key的列表中


    // key到val的映射，后面成为KV表
    HashMap<Integer, Integer> keyToVal;

    // key到freq的映射，后面成为KF表
    HashMap<Integer, Integer> keyToFreq;

    // freq到key列表的映射，后面成为FK表
    HashMap<Integer, LinkedHashSet<Integer>> freqToKeys;
    //记录最小的freq
    int minFreq;
    // 记录LFU缓存的最大容量
    int cap;

    // 构造容量为capacity的缓存
    public LFUCache(int capacity) {
        keyToVal = new HashMap<>();
        keyToFreq = new HashMap<>();
        freqToKeys = new HashMap<>();
        this.cap = capacity;
        this.minFreq = 0;
    }

    // 在缓存中查找key
    public int get(int key) {
        if (!keyToVal.containsKey(key)) {
            return -1;
        }
        //增加key对应的freq
        increaseFreq(key);
        return keyToVal.get(key);
    }

    // 将key存入缓存
    public void put(int key, int val) {
        if (this.cap <= 0) {
            return;
        }

        /**
         * 若key已存在，修改对应的val即可
         */
        if (keyToVal.containsKey(key)) {
            keyToVal.put(key, val);
            // key对应的freq +1
            increaseFreq(key);
            return;
        }

        /**
         * key不存在，需要插入
         * 容量满的话，需要淘汰一个freq最小的key
         */
        if (this.cap <= keyToVal.size()) {
            revmoeMinFreq();
        }

        /**
         * 插入key和val，对应的freq为1
         */
        // 插入KV表
        keyToVal.put(key, val);
        // 插入KF表
        keyToFreq.put(key, 1);
        // 插入FK表
        freqToKeys.putIfAbsent(1, new LinkedHashSet<Integer>());
        freqToKeys.get(1).add(key);
        // 插入新key后最小的freq肯定是1
        this.minFreq = 1;

    }

    private void revmoeMinFreq() {
        // freq最小的key列表
        LinkedHashSet<Integer> keyList = freqToKeys.get(this.minFreq);
        // 其中最先被插入的那个key就是被淘汰的key
        int deletedKey = keyList.iterator().next();
        // 更新FK表
        keyList.remove(deletedKey);
        if (keyList.isEmpty()) {
            freqToKeys.remove(this.minFreq);
        }
        // 更新KV表
        keyToVal.remove(deletedKey);
        // 更新KF表
        keyToFreq.remove(deletedKey);
    }

    private void increaseFreq(int key) {
        int freq = keyToFreq.get(key);
        // 更新KF表
        keyToFreq.put(key, freq + 1);
        // 更新FK表
        // 将key从freq对应的列表中删除
        freqToKeys.get(freq).remove(key);
        // 将key加入freq+1 对应的列表
        freqToKeys.putIfAbsent(freq + 1, new LinkedHashSet<Integer>());
        freqToKeys.get(freq + 1).add(key);

        //如果freq对应的列表空了，移除这个freq
        if (freqToKeys.get(freq).isEmpty()) {
            freqToKeys.remove(freq);
            //如果这个freq恰好是minFreq，更新minFreq
            if (freq == this.minFreq) {
                this.minFreq++;
            }
        }
    }
}

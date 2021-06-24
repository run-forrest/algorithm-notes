### 2Sum I
> 输入一个数组nums和一个整数target，可以保证数组中存在两个数的和为target，请返回两个数的索引
```
//输入nums= [3,1,3,6],target = 6 算法返回[0, 2]
int[] twoSum()

    int[] twoSum(int[] nums, int target) {
        //穷举这两个数的所有可能
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[j] == target - nums[i]) {
                    return new int[]{i, j};
                }
            }
        }
        //不存在
        return new int[]{-1, -1};
    }

    /**
     * 这种解法非常直接，时间复杂度为O(N2),可以通过哈希表记录元素值到索引的映射
     */
    int[] twoSum2(int[] nums, int target) {
        int n = nums.length;
        HashMap<Integer, Integer> index = new HashMap<>();
        //构造一个哈希表：元素映射到相应的索引
        for (int i = 0; i < n; i++) {
            index.put(nums[i], i);
        }
        for (int i = 0; i < n; i++) {
            int other = target - nums[i];
            //如果other存在且不是nums[i]本身
            if (index.containsKey(other) && index.get(other) != i) {
                return new int[]{i, index.get(other)};
            }
        }
        return new int[]{-1, -1};
    }
```

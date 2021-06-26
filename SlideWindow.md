### 滑动窗口
```
void slidingWindow(String s, String t) {
        HashMap<Character, Integer> need = new HashMap();
        HashMap<Character, Integer> window = new HashMap();
        for (char c : t.toCharArray()) {
            need.put(c, need.getOrDefault(c, 0) + 1);
        }
        int left = 0, right = 0;
        int valid = 0;
        while (right < s.length()) {
            //c 是将要移入窗口的字符
            char c = s.charAt(right);
            //右移窗口
            right++;
            //进行窗口内数据的一系列更新
            System.out.printf("window:[%d,%d)\n", left, right);

            //判断左侧窗口是否收缩
            while (window need shrinks){
                //d是将要移除窗口的字符
                char d = s.charAt(left);
                //左移窗口
                left++;
                //进行窗口内数据的一系列更新
                  d
            }
        }
    }
```

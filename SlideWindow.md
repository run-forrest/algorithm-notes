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
                  
            }
        }
    }
    
   //输入S=“ADBECFEBANC” 算法应该返回“BANC”
   String minWindow(String s, String t) {
        HashMap<Character, Integer> need = new HashMap<>();
        HashMap<Character, Integer> window = new HashMap<>();

        for (char c : t.toCharArray()) {
            need.put(c, need.getOrDefault(c, 0) + 1);
        }
        int left = 0, right = 0;
        int valid = 0;
        //记录最小覆盖子串的其实索引和长度
        int start = 0, len = Integer.MAX_VALUE;
        while (right < s.length()) {
            //c是将要移入窗口
            char c = s.charAt(right);
            //右移窗口
            right++;
            //进行窗口内数据的一系列更新
            if (need.containsKey(c)) {
                window.put(c, window.getOrDefault(c, 0) + 1);
                if (window.get(c).equals(need.get(c))) {
                    valid++;
                }
                //判断左侧窗口是否要收缩
                while (valid == need.size()) {
                    //在这里更新最小子串
                    if (right - left < len) {
                        start = left;
                        len = right - left;
                    }
                    // d是将要移除窗口的字符
                    char d = s.charAt(left);
                    left++;
                    //进行窗口内数据的一系列更新
                    if (need.containsKey(d)) {
                        if (window.get(d).equals(need.get(d))) {
                            valid--;
                        }
                        window.put(d, window.getOrDefault(d, 0) - 1);

                    }
                }
            }
        }
        return len == Integer.MAX_VALUE ? "" : s.substring(start,start+len);
    }
```

## 二叉树-二叉搜索树(Binary search Tree 简称BST)

>  定义：一个二叉树中，任意节点的值要大于等于左子树所有节点的值; 中序遍历时，为升序数组

### 判断BST的合法性：

```
        5
      2    6
    1   4    7
       3 
       
  boolean isValidBST(TreeNode root) {
    return isValidBST(root, null, null);
  }
  
  boolean isValidBST(TreeNode root, TreeNode min, TreeNode max) {
    if (root == null) {
      return true;
    }
    if (min != null && root.val <= min.val) {
      return false;
    }
    if (max != null && root.val >= max.val) {
      return false;
    }
    return isValidBST(root.left, min, root) && isValidBST(root.right, root, max);
  }
  // 这样相当于给子树上的所有节点添加了min,max边界，约束root的左子树节点值不超过root值，右子树节点值不小于root节点值
      
```

### 查找一个数
```
boolean isInBST(TreeNode root, int target) {
    if (root == null) {
        return false;
    }
    if (root.val ==target) {
        return true;
    }
    //根据BST特性判断大小
    if (root.val < traget) {
        return isInBST(root.right, target);
    }
    if (root.val > target) {
        return isInBST(root.left, target);
    }
}

```

### BST中插入一个数
```
TreeNode insertIntoBST(TreeNode root, int val) {
    // 找到空位置插入新节点
    if (root == null) {
        return new TreeNode(val);
    }
    //如果已经存在，则不要再重复插入，直接返回
    if (root.val == val) {
        return root
    }
    //val小，则应该插到左子树上
    if (root.val < val) {
        root.right = insertInBST(root.right, val);
    }
    //val大，则插到右子树上
    if (root.val > val) {
        root.left = insertInBST(root.left, val);
    }
    
   return root;
}

```

### 在BST中删除一个数

这个问题有点复杂，但是流程还是：先找，后改
```
Tree deleteNode(Treee root, int key) {
    if (root == null) return null;
    if (root.val == key) {
        //找到了，进行删除
        //1.恰好是末端节点、
        //2.只有一个非空子阶段，让自己的子node接替自己
        if(root.left == null) {
            return root.right;
        }
         if(root.right == null) {
            return root.left;
        }
        //3. 有两个子节点，那就必须找左子树上最大节点，或者右子树上最小节点接替自己
        //这里使用右子树最小节点
        TreeNode minNode = getMin(root.right);
        root.val = minNode.val;
        root.right = deleteNode(root.right, minNode.val);
    } else if (root.val > key) {
        //去左子树上寻找
        root.left = deleteNode(root.left, key);
    } else if (root.val < key) {
        //去右子树上寻找
        root.right = deleteNode(root.right, key);
    }
    return root;
}

TreeNode getMin(TreeNode node) {
    //BST最左边的就是最小的
    while (node.left != null) {
        node = node.left;
        return node;
    }
}
```

### Complete Binary Tree 完全二叉树
> 定义：每一层都是紧凑的靠左排列
> 特例：满二叉树，特殊的完全二叉树，Perfect Binary Tree,Full Binary Tree
```
publi int countNodes(TreeNode root) {
  TreeNode l = root, right = root;
  int hl = 0. rl = 0;
  while (l != null) {
    l = l.left;
    hl++;
  }
  while (r != null) {
    r = r.right;
    hr++;
  }
  //如果左右子树的高度相同，说明是一棵满二叉树
  if (hl == rl) {
    return (int)Math.pow(2,hl)-1;
  }
  //如果左右子树不相同，则按普通二叉树的逻辑计算
  return 1 + countNodes(root.left) + countNodes(root.right);
}
```
时间复杂度为O(logN*logN),因为while需要logN的时间，而递归里面只有一个会真递归下去，另一个一定会触发hl==hr，是一棵满二叉树

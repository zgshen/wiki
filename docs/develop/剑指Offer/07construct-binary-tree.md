# 重建二叉树

输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

- 前序遍历：根节点->左节点->右节点
- 中序遍历：左节点->根节点->右节点
- 后序遍历：左节点->右节点->根节点

前序遍历的首元素就是二叉树的根节点，再根据前序和中序遍历的特点，在中序遍历中找到根节点的索引，就可以将中序遍历结果的数组分成左子树、根节点和右子树三部分。

然后就能确定树的根节点、左子树的根节点、右子树的根节点三个节点。

再根据分治算法的思想，使用同样的方法继续划分左右子树。

- 时间复杂度O(N)：遍历数组存到哈希表O(N)，递归N个节点，每个O(1)，也是O(N)。
- 空间复杂度O(N)：哈希表O(N)，当二叉树是链表这种极端情况下递归也是O(N)。

```java
class Solution {

    HashMap<Integer, Integer> dic  = new HashMap<>();
    int[] preorder;

    public TreeNode buildTree(int[] preorder, int[] inorder) {    
        this.preorder = preorder;
        // 中序遍历数组存到哈希表，便于快速查找根节点
        for (int i=0; i<preorder.length; i++) dic.put(inorder[i], i);
        return recur(0, 0, preorder.length-1);
    }

    /**
     * @param root 前序根节点
     * @param left 中序左节点
     * @param right 中序右节点
     */
    private TreeNode recur(int root, int left, int right) {
        if (left>right) return null;
        TreeNode node = new TreeNode(preorder[root]);
        int subRoot = dic.get(preorder[root]);
        node.left = recur(root+1, left, subRoot-1);
        // 前序右子树根节点下标=左子树长度(subRoot-left)+根节点+1
        node.right = recur(subRoot-left+root+1, subRoot+1, right);
        return node;
    }
}
```

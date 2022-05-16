
# 矩阵中的路径

给定一个 m x n 二维字符网格 board 和一个字符串单词 word 。如果 word 存在于网格中，返回 true ；否则，返回 false 。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

# 分析

矩阵搜索问题，使用深度优先搜索（DFS）和剪枝处理。

遍历矩阵，以矩阵每个元素为起点匹配字符串，不匹配返回false，匹配则往元素的四个方向继续遍历匹配，遍历过的元素需要标记为空字符以防止重复匹配。

当匹配的串长度等于字符串长度时，意味着匹配成功。每次不管匹配成功与否，最终返回需要把空字符元素置为原来的值。

- 时间复杂度 O(\(3^K MN\))
- 空间复杂度 O(K)，递归深度不超过K，函数调用栈空间O(K)

```java
public boolean exist(char[][] board, String word) {
    char[] words = word.toCharArray();
    for (int i=0; i<board.length; i++) {
        for (int j=0; j<board[0].length; j++) {
            if(dfs(board, words, i, j, 0)) return true;
        }
    }
    return false;
}

private boolean dfs(char[][] board, char[] words, int i, int j, int k) {
    // 数组越界或者字符不匹配返回 false
    if (i<0 || j<0 || i>=board.length || j>=board[0].length || board[i][j]!=words[k]) return false;
    // 匹配的长度等于字符串长度即匹配成功
    if (k == words.length - 1) return true;
    board[i][j] = '\0';
    // 向元素的上下左右继续遍历
    boolean res = dfs(board, words, i+1, j ,k+1) || dfs(board, words, i-1, j, k+1) || 
        dfs(board, words, i, j+1, k+1) || dfs(board, words, i, j-1, k+1);
    // 然后返回前要把当前元素置为原本的值
    board[i][j] = words[k];
    return res;
}
```

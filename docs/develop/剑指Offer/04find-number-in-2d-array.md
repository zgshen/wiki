
# 二位数组的查找

在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个高效的函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

Write an efficient algorithm that searches for a value target in an m x n integer matrix matrix. This matrix has the following properties:

Integers in each row are sorted from left to right.
The first integer of each row is greater than the last integer of the previous row.

```
Exp1：
Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
Output: true

Exp2:  
Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
Output: false
```

从右上角向左下角遍历。右上角的数字是每一行的最大值和每一列的最小值，数字比目标值小则可以淘汰一行，比目标值大则可以淘汰一列。

```java
public boolean searchMatrix(int[][] matrix, int target) {
    if (matrix==null || matrix[0].length==0) return false;
    int m = 0;
    int n = matrix[0].length-1;
    
    while (m<matrix.length && n>=0) {
        if (matrix[m][n] == target) {
            return true;
        } else if (matrix[m][n] < target) {
            m++;
        } else {
            n--;
        }
    }
    return false;
}
```

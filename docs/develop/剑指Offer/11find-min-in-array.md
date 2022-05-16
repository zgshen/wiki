
# 旋转数组中的最小数字

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。

给你一个可能存在 重复 元素值的数组 numbers ，它原来是一个升序排列的数组，并按上述情形进行了一次旋转。请返回旋转数组的最小元素。

- 时间复杂度 O(\(\log_2N\))：二分法 O(\(\log_2N\))，在特例情况下（例如 [1, 1, 1, 1]），会退化到 O(N)。
- 空间复杂度 O(1)：i，j，m 变量使用常数大小的额外空间。

```java
public int findMin(int[] nums) {
    int i=0, j=nums.length-1;
    while (i<j) {
        int m = (i+j)/2;
        if (nums[m] > nums[j]) i=m+1;
        else if (nums[m] < nums[j]) j=m;
        //else j--;
        // 或者直接使用线性查找
        else {
            int x=i;
            for (int k=0; k<j; k++) {
                if (nums[k] < nums[x]) x=k;
            }
            return nums[x];
        }
    }
    return nums[i];
}
```

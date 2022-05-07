
# 数组中重复的数字

找出数组中重复的数字。

在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。


使用集合的方法。
```java
public int findDuplicate(int[] nums) {
    Set<Integer> set = new HashSet<>();
    for (int num : nums) {
        if (set.contains(num)) return num;
        set.add(num);
    }
    return -1;
}
```

原地交换法。
```java
public int findDuplicate(int[] nums) {
    if (nums == null || nums.length==0) return -1;
    int index = 0;
    while (index < nums.length) {
        if (nums[index] == index) {
            index++;
            continue;
        }
        if (nums[index] == nums[nums[index]]) return nums[index];
        int tmp = nums[index];
        nums[index] = nums[tmp];
        nums[tmp] = tmp;   
    }
    return -1;
}
```

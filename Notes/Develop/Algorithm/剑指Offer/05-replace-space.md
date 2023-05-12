
# 替换空格

请实现一个函数，把字符串 s 中的每个空格替换成"%20"。

限制：0 <= s 的长度 <= 10000

- 时间复杂度O(N)：遍历字符数组
- 空间复杂度O(N)：StringBuilder 使用的空间

```java
public String replaceSpace(String s) {
    StringBuilder sb = new StringBuilder();
    for (Character c : s.toCharArray()) {
        if (c == ' ') sb.append("%20");
        else sb.append(c);
    }
    return sb.toString();
}
```
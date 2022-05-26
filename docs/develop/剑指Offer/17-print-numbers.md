
# 打印从1到最大的n位数

输入数字 n，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。

#### 分析

不考虑大数就只是个简单的循环。

```java
class Solution {
    public int[] printNumbers(int n) {
        int end = (int)Math.pow(10, n) - 1;
        int[] res = new int[end];
        for(int i = 0; i < end; i++)
            res[i] = i + 1;
        return res;
    }
}
```

考虑大数，用字符串表示数字。

```java
public class BigNumber {

    StringBuilder res;
    int n;
    char[] num, loop = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'};
    public String printNumbers(int n) {
        this.n = n;
        res = new StringBuilder(); // 数字字符串集
        num = new char[n]; // 定义长度为 n 的字符列表
        dfs(0); // 开启全排列递归
        res.deleteCharAt(res.length() - 1); // 删除最后多余的逗号
        return res.toString(); // 转化为字符串并返回
    }
    void dfs(int x) {
        if(x == n) { // 终止条件：已固定完所有位
            res.append(String.valueOf(num) + ","); // 拼接 num 并添加至 res 尾部，使用逗号隔开
            return;
        }
        for(char i : loop) { // 遍历 ‘0‘ - ’9‘
            num[x] = i; // 固定第 x 位为 i
            dfs(x + 1); // 开启固定第 x + 1 位
        }
    }
}
```

返回整形数组的解法。

```java
public class BigNumber {
    // 结果
    int[] res;
    // 计数 位数
    int count = 0, n;
    // 数字 组成数字的数字字符
    char[] num, loop = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'};
    // 数字字符串0的个数
    int nine, start;
    public int[] printNumbers(int n) {
        this.n = n;
        // 数组长度
        res = new int[(int) Math.pow(10, n) - 1];
        // 数字，最长有n位
        num = new char[n];
        start = n - 1;
        dfs(0);
        return res;
    }

    void dfs(int x) {
        if (x == n) {
            // n 位组装好成一个数字就输出
            String s = String.valueOf(num).substring(start);
            if (!"0".equals(s)) res[count++] = Integer.parseInt(String.valueOf(s));
            if (n - start == nine) start--;
            return;
        }
        for (char c : loop) {
            // 数字长度，与递归深度有关，用来计算start
            if (c == '9') nine++;
            // 数字第x位上的数
            num[x] = c;
            // 下一个高位的数
            dfs(x+1);
        }
        nine--;
    }
}
```

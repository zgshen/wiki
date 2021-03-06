
# 斐波那契数列

写一个函数，输入 n ，求斐波那契（Fibonacci）数列的第 n 项（即 F(N)）。斐波那契数列的定义如下：

F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

#### 分析

简单的递归方法，两数相加作为结果返回就好。缺点是当n的值越大，效率越低，时间复杂度是O(2^N)，1变2，2变4...就是2的n次方。空间复杂度O(N)。
```java
public int fib(int n) {
    if (n == 0 || n == 1) return n;
    return fib(n-1) + fib(n-2);
}
```

动态规划算法（dp），时间复杂度O(N)，空间复杂度O(1)。
```java
public int fib(int n) {
    int a = 0, b = 1, sum;
    for(int i = 0; i < n; i++){
        sum = (a + b) % 1000000007;
        a = b;
        b = sum;
    }
    return a;
}
```

# 青蛙跳台阶问题

一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 n 级的台阶总共有多少种跳法。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

#### 分析

不管青蛙怎么跳，最后一步只有两种情况：跳上一级台阶或者两级台阶。
- 当最后跳上一级台阶时，剩余 n-1 个台阶，此时有 f(n-1) 种方法；
- 当最后跳上两级台阶时，剩余 n-2 个台阶，此时有 f(n-2) 种方法。

那么 f(n) = f(n-1) + f(n-2)，与斐波那契数列的类似，知识初始值不同而已。

f(0)=1，f(1)=1，f(2)=2 。

```java
public int numWays(int n) {
    int a=1, b=1, sum=0;
    for (int i=0; i<n; i++) {
        sum = (a+b)%1000000007;
        a = b;
        b = sum;
    }
    return a;
}
```
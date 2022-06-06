
# 剪绳子

给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m-1] 。请问 k[0]*k[1]*...*k[m-1] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

提示：2 <= n <= 58

### 分析

使用动态规划比较容易理解。。

参考 https://leetcode.cn/problems/jian-sheng-zi-lcof/solution/mian-shi-ti-14-i-jian-sheng-zi-tan-xin-si-xiang-by/1391865

1. 状态定义:dp[i]为长度为i的绳子剪成m段最大乘积为dp[i]

2. 状态转移:dp[i]有两种途径可以转移得到
    2.1 剪成两段。前面单独成一段,后面剩下的单独成一段,乘积为j*(i-j),乘积个数为2
    2.2 大于两段。第一次剪去了j，剩余的就是dp[i-j]，乘积dp[i-j]*j

    两种情况中取大的值作为dp[i]的值,同时应该遍历所有j,j∈[2,i-2],j从2开始，j=1做乘积没意义

3. 初始化:初始化dp[1]=1即可

4. 遍历顺序:显然为正序遍历

5. 返回坐标:返回dp[n]

```java
class Solution {
    public int cuttingRope(int n) {
        //因为dp下表从0开始，dp[0]是第一个，dp[n]是n+1个
        int dp[] = new int[n+1];
        //2 <= n <= 58，不管dp[1]
        //m段，m>1，1x1=1
        dp[2] = 1;
        //i表示绳子长度，从3开始，小于3不用算了乘积都是1
        for (int i=3; i<=n; i++) {
            //j表示第一次剪去的长度，剩余i-j
            //小于等于 i-2 是因为最后是 j*1 等于 j，没意义
            for (int j=1; j<=i-2; j++) {
                //两段直接相乘，或者i-j继续分段，比较取最大值
                int tmp = Math.max(dp[i-j]*j, (i-j)*j);
                //和前一个dp比较取最大值
                dp[i] = Math.max(tmp, dp[i]);
            }
        }
        return dp[n];
    }
}
```

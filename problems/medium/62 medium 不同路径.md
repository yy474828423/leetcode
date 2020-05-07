#题目描述
一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。  
机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。  
问总共有多少条不同的路径？

## 示例 1:

输入: m = 3, n = 2  
输出: 3  
解释:  
从左上角开始，总共有 3 条路径可以到达右下角。  
1. 向右 -> 向右 -> 向下
2. 向右 -> 向下 -> 向右
3. 向下 -> 向右 -> 向右


## 示例 2:

输入: m = 7, n = 3  
输出: 28

## 提示：


	1 <= m, n <= 100  
	题目数据保证答案小于等于 2 * 10 ^ 9


# 解题思路
## plan 1
组合  
机器人一定会走m+n-2步，即从m+n-2中挑出m-1步向下走
即C（（m+n-2），（m-1））。

```
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        #cm+n-2 m-1
        return int(factorial(m+n-2)//factorial(m-1)/factorial(n-1))
    def factorial(x):
        if x ==1:return 1
        ans = x*factorial(x-1)
        return ans
```
## plan 2
递归
```
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        if m ==1 or n==1:
            return 1

        ans = 1
        ans*= self.uniquePaths(m-1,n) + self.uniquePaths(m,n-1)
        return ans
```
超时

递归优化 缓存
```
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        return self.recur(m,n,{})
    def recur(self,m,n,dic):
        if n==1 or m==1:
            return 1
        if (m,n) in dic:4
            return dic[(m,n)]
        if (n,m) in dic:
            return dic[(n,m)]
        dic[(m,n)]=self.recur(m,n-1,dic)+self.recur(m-1,n,dic)   
        return dic[(m,n)]
```

## plan 3
动态规划

令 dp[i][j] 是到达 i, j 最多路径  
动态方程：dp[i][j] = dp[i-1][j] + dp[i][j-1]  
注意，对于第一行 dp[0][j]，或者第一列 dp[i][0]，由于都是在边界，所以只能为 1  
时间复杂度：O(m∗n)  
空间复杂度：O(m∗n)

```
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp = [[1]*n] + [[1]+[0] * (n-1) for _ in range(m-1)]
        #print(dp)
        for i in range(1, m):
            for j in range(1, n):
                dp[i][j] = dp[i-1][j] + dp[i][j-1]
        return dp[-1][-1]
```

优化1：空间复杂度 O(2n)  
由于dp[i][j] = dp[i-1][j] + dp[i][j-1]，因此只需要保留当前行与上一行的数据 (在动态方程中，即pre[j] = dp[i-1][j])，两行，
```
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        pre = [1] * n
        cur = [1] * n
        for i in range(1, m):
            for j in range(1, n):
                cur[j] = pre[j] + cur[j-1]
            pre = cur[:] # 前一行赋予pre
        return pre[-1]
```

优化2：空间复杂度 O(n)  
cur[j] += cur[j-1], 即cur[j] = cur[j] + cur[j-1] 等价于思路二-->> cur[j] = pre[j] + cur[j-1]  
等号右边分别是该位置上边的值和左边的值  
一维的dp数组定义为当前行中，每个元素对应的步数。   
对于二维数组dp[i][j] = dp[i-1][j] + dp[i][j-1]
对于一维数组， 对于第i行来说，dp[j] = dp[j] + dp[j-1], 等号右边的未赋值之前的dp[j]就是上一行的第j个数据对应的步数，即dp[i-1][j]  
等号右边的dp[j-1]是已经更新过的本行的第j-1个数据对应的步数，即dp[i][j-1]  
则，本行的dp[j] = 上一行的dp[j] + 本行的dp[j-1]， 所以dp[j] = dp[j] + dp[j-1].
```
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        cur = [1] * n
        for i in range(1, m):
            for j in range(1, n):
                cur[j] += cur[j-1]
        return cur[-1]

```

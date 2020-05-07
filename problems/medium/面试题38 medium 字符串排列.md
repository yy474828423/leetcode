# 题目描述
输入一个字符串，打印出该字符串中字符的所有排列。
你可以以任意顺序返回这个字符串数组，但里面不能有重复元素。

## 示例:

输入：s = "abc"
输出：["abc","acb","bac","bca","cab","cba"]


## 限制：
1 <= s 的长度 <= 8



# 解题思路
递归

```
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        ans = []
        if len(s)<2:
            return  s
        else:
            for i in range(len(s)):
                for v in permutation(s[:i]+s[i+1:]):
                # 对于每个permute 返回值都是一个字符串的列表，
                # 遍历这个列表，
                # 其中每个值加上前一个值
                    ans.append(s[i]+v)
        return ans
```

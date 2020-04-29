# 题目描述
42. 接雨水

给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

## 示例:
![permutation](./resourses/rainwatertrap.PNG)

输入  [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。

输入: [0,1,0,2,1,0,1,3,2,1,2,1]

输出: 6


# 解题思路


```
class Solution:
    def trap(self, height: List[int]) -> int:
```

## plan 1


```
def method1(height):
    max_left = max_right = ans = 0
    for i in range(1,len(height)):
        max_left = max(height[:i+1])
        max_right = max(height[i:])
        ans += min(max_left,max_right)-height[i]
    return ans
```
### 执行效率


## plan 2
```
def method1(height):
    l = len(height)
    ans1 = ans2 = 0
    max_left = max_right = 0
    for i in range(l):
        if height[i]>max_left:
            max_left = height[i]
            ans1+= max_left
        else:
            ans1+= max_left
        if height[-i-1]>max_right:
            max_right = height[-i-1]
            ans2+=max_right
        else:
            ans2+=max_right
    return ans1+ans2-max(height)*l-sum(height)
```

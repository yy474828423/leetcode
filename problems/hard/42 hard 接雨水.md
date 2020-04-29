# 题目描述
42. 接雨水

给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

## 示例:
![permutation](../resourses/rainwatertrap.PNG)

输入  [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。

输入: [0,1,0,2,1,0,1,3,2,1,2,1]

输出: 6


# 解题思路


```
class Solution:
    def trap(self, height: List[int]) -> int:
```

## plan 1
直观想法
直接按问题描述进行。对于数组中的每个元素，我们找出下雨后水能达到的最高位置，等于两边最大高度的较小值减去当前高度的值。

```
max_left = max_right = ans = 0
for i in range(1,len(height)):
    max_left = max(height[:i+1])
    max_right = max(height[i:])
    ans += min(max_left,max_right)-height[i]
return ans
```
### 执行效率
执行用时 :1932 ms, 在所有 Python3 提交中击败了5.01%的用户
内存消耗 :14 MB 在所有 Python3 提交中击败了8.00%的用户

## plan 2
动态计算 遍历2遍 O(n)
![permutation](../resourses/53ab7a66023039ed4dce42b709b4997d2ba0089077912d39a0b31d3572a55d0b-trapping_rain_water.PNG)
```
if height:
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
else:
    return 0
```
### 效率
执行用时 :52 ms, 在所有 Python3 提交中击败了63.43%的用户
内存消耗 :14.2 MB, 在所有 Python3 提交中击败了8.00%的用户
## plan 3
plan2 的升级版 双指针 只遍历一遍
```
left_index =0
right_index =len(height)-1
max_left = max_right = ans = 0
while (left_index < right_index+1):
    if max_left < max_right:
        if max_left<height[left_index]:
            max_left = height[left_index]
        left_index +=1
        ans+=max_left
    else:
        if max_right <height[right_index]:
            max_right = height[right_index]
        right_index -= 1
        ans += max_right
return ans-sum(height)
```

### 效率
执行用时 :48 ms, 在所有 Python3 提交中击败了77.39%的用户
内存消耗 :14.1 MB, 在所有 Python3 提交中击败了8.00%的用户

## plan 4
栈

## plan 5
一行解法 动态计算变种 not pythonic
```
class Solution:
    def trap(self, height: List[int]) -> int:
        return sum(map(min, accumulate(height, max), list(accumulate(height[::-1], max))[::-1])) - sum(height)
```

# 题目描述
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

 

示例:

给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]

# 解题思路

## plan 1
 双重循环（略），内循环切片
 
 ## plan 2
 利用字典，将使用过的值放到字典里，利用target-num的值对其进行查询
    
```
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        x = {}
        for i in range(len(nums)):
            another = target - nums[i]
            if another in x:
                return [x[another],i]
            else:
                x[nums[i]]=i
```

## plan 3 
每次用target-num的值在list里索引
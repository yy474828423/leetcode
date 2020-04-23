# 题目描述

>506. 相对名次

>给出 N 名运动员的成绩，找出他们的相对名次并授予前三名对应的奖牌。前三名运动员将会被分别授予 “金牌”，“银牌” 和“ 铜牌”（"Gold Medal", "Silver Medal", "Bronze Medal"）。

>(注：分数越高的选手，排名越靠前。)

示例 1:

输入: [5, 4, 3, 2, 1]
输出: ["Gold Medal", "Silver Medal", "Bronze Medal", "4", "5"]
解释: 前三名运动员的成绩为前三高的，因此将会分别被授予 “金牌”，“银牌”和“铜牌” ("Gold Medal", "Silver Medal" and "Bronze Medal").
余下的两名运动员，我们只需要通过他们的成绩计算将其相对名次即可。
提示:

N 是一个正整数并且不会超过 10000。
所有运动员的成绩都不相同。

# 解题思路

首先这个题需要你排序，因为返回的是原列表的顺序值，所以还需要记下索引，按照索引，将对应值修改为排名值

## plan 1
因为分数是唯一的
利用字典构建分数与排名的hash索引
'''
class Solution:
    def findRelativeRanks(self, nums: List[int]) -> List[str]:
        reverse_num = sorted(nums,reverse=True)
        map_dic = {}

        for i,val in enumerate(reverse_num):
            if i ==0:
                map_dic[val] = 'Gold Medal'
            elif i ==1:
                map_dic[val] = 'Silver Medal'
            elif i ==2:
                map_dic[val] = 'Bronze Medal'
            else:
                map_dic[val] = str(i+1)
        res = []
        for num in nums:
            res.append(map_dic[num])
        return res
'''
### 执行效率
84 ms 85.40%	14.6 MB 100.00%

## plan 2
使用列表排序方法，将索引与键值同时排序
'''
class Solution:
    def findRelativeRanks(self, nums: List[int]) -> List[str]:
		nums_copy = nums.copy()
        length = len(nums_copy)
        if length <2:
            return ["Gold Medal"]
        else:
            sort_v= sorted(list(enumerate(nums_copy)),key=lambda x:x[1],reverse=True)
            nums_copy[sort_v[0][0]] = "Gold Medal"
            nums_copy[sort_v[1][0]] = "Silver Medal"
            if length >2:
                nums_copy[sort_v[2][0]] = "Bronze Medal"

                for i in range(3,length):
                    nums_copy[sort_v[i][0]] = str(i+1)
        return nums_copy
'''


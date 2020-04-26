# 题目描述
46. 全排列

给定一个 没有重复 数字的序列，返回其所有可能的全排列。

## 示例:

输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]


# 解题思路
搜索问题 dfs/bfs

```
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
```

## plan 1
递归
```
res = []
l = len(nums)
if l<2:
	return [nums]
else:
	for i in range(l):
		others = nums[:i]+nums[i+1:]
		for j in self.permute(others):
			res.append([nums[i]]+j)
return res # return res.append([nums[i]]+j) 会发生神奇的事情哦
```
### 执行效率
执行用时 :44 ms, 在所有 Python3 提交中击败了63.51%的用户
内存消耗 :13.7 MB, 在所有 Python3 提交中击败了5.41%的用户

## plan 2
回溯
```
return str(num).replace("6","9",1)
```
### 执行效率
执行用时 :44 ms, 在所有 Python3 提交中击败了26.43%的用户
内存消耗 :13.7 MB, 在所有 Python3 提交中击败了100.00%的用户

## plan 3
借助列表
```
list_num = list(str(num))
for i,val in enumerate(list_num):
	if val =='6':
		list_num[i] = '9'
		break
return int(''.join(list_num))
```
### 执行效率
执行用时 :44 ms, 在所有 Python3 提交中击败了26.55%的用户
内存消耗 :13.7 MB, 在所有 Python3 提交中击败了100.00%的用户

## plan 4
打表
```
dic = {6:9, 9:9, 66:96, 69:99, 96:99, 99:99, 666:966, 669:969, 696:996, 699:999, 966:996, 969:999, 996:999, 999:999, 6666:9666, \
            6669:9669, 6696:9696, 6699:9699, 6966:9966, 6969:9969, 6996:9996, 6999:9999, 9666:9966, 9669:9969, 9696:9996, \
            9699:9999, 9966:9996, 9969:9999, 9996:9999, 9999:9999
            }
return dic[num]
```
### 执行效率
执行用时 :36 ms, 在所有 Python3 提交中击败了59.44%的用户
内存消耗 :13.8 MB, 在所有 Python3 提交中击败了100.00%的用户

## plan 5
数学 从后向前 如果%10^t ==6 将返回值修改为num + t * 3
```
res = num
n = num
t = 1
while num // t > 0:  
	if n % 10 == 6:
		res = num + t * 3
	t *= 10
	n //= 10
return res
```
### 执行效率
执行用时 :36 ms, 在所有 Python3 提交中击败了59.44%的用户
内存消耗 :13.8 MB, 在所有 Python3 提交中击败了100.00%的用户

## plan 6
数学 从前向后 求出输入数据位数 
```
num_len = len(str(num))
n = num
for i in range(num_len-1,-1,-1):
	x = n//(10**i)
	if x == 6:
		return num + 10**i*3
	else:
		n -= 10**i *x
return num
```
### 执行效率
执行用时 :40 ms, 在所有 Python3 提交中击败了39.86%的用户
内存消耗 :13.5 MB, 在所有 Python3 提交中击败了100.00%的用户

# 题目描述

290. 单词规律

给定一种规律 pattern 和一个字符串 str ，判断 str 是否遵循相同的规律。
这里的 遵循 指完全匹配，例如， pattern 里的每个字母和字符串 str 中的每个非空单词之间存在着双向连接的对应规律。
## 示例1:
输入: pattern = "abba", str = "dog cat cat dog"
输出: true
## 示例 2:
输入:pattern = "abba", str = "dog cat cat fish"
输出: false
## 示例 3:
输入: pattern = "aaaa", str = "dog cat cat dog"
输出: false
## 示例 4:
输入: pattern = "abba", str = "dog dog dog dog"
输出: false
## 说明:
你可以假设 pattern 只包含小写字母， str 包含了由单个空格分隔的小写字母。    


# 解题思路
* 判断两遍对应关系，首先想到了字典于是有了方法1
* 本题有一些坑点：两边长度是否一致、反映射是否正确


```
class Solution:
    def wordPattern(self, pattern: str, str: str) -> bool:
```

## plan 1
双字典映射 18行
```
list_str = str.split(' ')
l1 = len(pattern)
l2 = len(list_str)
if l1 != l2:
   return False
else:
   d1,d2 = {},{}
   for i in range(l1):
       key1 = pattern[i]
       key2 = list_str[i]
       if key1 in d1 and d1[key1] != key2:
           return False
       if key2 in d2 and d2[key2] !=key1:
           return False
       else:
           d2[key2] = key1
           d1[key1] = key2
   return True
```
### 执行效率
执行用时 :28 ms, 在所有 Python3 提交中击败了95.54%的用户
内存消耗 :13.5 MB, 在所有 Python3 提交中击败了10.00%的用户

## plan 2
一行代码，使用map函数，此时x.index 返回的是个Function
本质是返回每个当前位置值的首个索引
两边索引一致的话 即为True
```
return (list(map(str.split(" ").index,str.split(" "))) == list(map(pattern.index,pattern)))
```
### 执行效率
执行用时 :52 ms, 在所有 Python3 提交中击败了15.79%的用户
内存消耗 :13.6 MB, 在所有 Python3 提交中击败了10.00%的用户

## plan 3
列表推导式，理论上效率比map函数能快一点
```
list_str = str.split(' ')
return [pattern.index(x) for x in pattern]==[list_str.index(x) for x in list_str]
```
### 执行效率
执行用时 :30 ms, 在所有 Python3 提交中击败了72.34%的用户
内存消耗 :13.7 MB, 在所有 Python3 提交中击败了10.00%的用户

这时候我们发现所有方法的内存消耗都比较高，怎么优化内存呢？

## plan 4
1.若不是则先判断单词s[k]是否已经储存在d(dict)里面：
  1）是，判断键值关系
  11）d[v]!=s[k] 返回F
  12）pass
  2）否，判断s[k]是否在d的值中，
  21）是 返回F
  22) 否 生成d[v] = s[k]
若以上情况均已排除，return true。

```
d = {}
s = str.split()
if len(pattern) != len(s):
    return False
for k, v in enumerate(pattern):
    if d.get(v):
        if d[v] != s[k]:
            return False
    else:
        if s[k] in d.values():
            return False
        d[v] = s[k]
return True
```
### 执行效率
执行用时 :44 ms, 在所有 Python3 提交中击败了35.76%的用户
内存消耗 :13.6 MB, 在所有 Python3 提交中击败了10.00%的用户

## plan 5
线性相关 线性：满足加法、乘法 
从非重复个数来做最终判断
```
s = str.split()
if len(s)!=len(pattern):
    return False
sp = [ x+y for x,y in zip(s,pattern)]
return len(set(s))==len(set(pattern))==len(set(sp))
```
### 执行效率
执行用时 :48 ms, 在所有 Python3 提交中击败了21.96%的用户
内存消耗 :13.4 MB, 在所有 Python3 提交中击败了10.00%的用户

## plan6
线性相关变种
```
s1=str.split(' ')

if len(pattern) != len(s1):
    return False
return len(set(zip(list(pattern),s1)))==len(set(s1))==len(set(pattern))
```

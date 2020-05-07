# 题目描述
给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

## 示例 1:

输入: "abcabcbb"

输出: 3

解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

## 示例 2:

输入: "bbbbb"

输出: 1

解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

## 示例 3:

输入: "pwwkew"

输出: 3

解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。



# 解题思路
滑动窗口
## plan 1
缓存一个到当前位置的最长的无重复字串tmp，以及当前情况下的最长值l
遍历S ，不断修改tmp和l

```
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        l = 0
        tmp =''
        if not s: return 0
        for i,val in enumerate(s):
            if val not in tmp:
                tmp+=val
            else:
                tmp = tmp[tmp.index(val)+1:]+val
            l = max(len(tmp),l)
        return l

```
### 效率
80ms
## plan 2
方法1中容器的伸缩涉及内存分配,所以方法2换成位置指针省掉了内存分配
```
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        l = 0
        left,right =0,0
        for i, c in enumerate(s):
            if c not in s[left:right]:
                right += 1
            else:
                left += s[left:right].index(c) + 1
                right += 1

            max_length = max(right - left, max_length)

        return max_length if max_length != 0 else len(s)


```
## plan 3
法2位置指针还是要扫描重复字符,所以方法3用Hash直接判断省掉了扫描开销.
```
class Solution(object):

    def lengthOfLongestSubstring(self, s: str) -> int:
        ignore_str_index_end = -1
        dic = {}        # 任意字符最后出现在索引的位置 - {字符: 字符索引值}
        max_length = 0  # 最长字符串长度

        for i, c in enumerate(s):
            # 如果字符索引值大于ignore_str_index_end，则字符c在需处理的范围内（补充说明请参考备注）
            if c in dic and dic[c] > ignore_str_index_end:
                # 先更新可抛弃字符串的索引尾值为字符c上一次的索引值
                ignore_str_index_end = dic[c]
                # 再更新字符c的索引值
                dic[c] = i
            # 否则，
            else:
                # 更新字符最近的索引位置
                dic[c] = i
                # 更新最大长度
                max_length = max(i - ignore_str_index_end, max_length)

        return max_length

```
备注
* 假设有字符串"abbcda", 观察可知最长不重复子串为"bcda"
* 根据编写的算法，在刚遍历至最后一个'a'时，dic['a']的值为0，此时ignore_str_index_end的值已经更新为索引1，索引1以及之前的字符都是出现在重复字符之前，不用再在运算中考虑的字符。
* ignore_str_index_end的注释，是可抛弃字符串的索引尾值，是双指针方法中左指针（起始针）的反面;

### 效率
60ms

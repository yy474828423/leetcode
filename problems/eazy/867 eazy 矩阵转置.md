# 题目描述


# 解题思路

## plan 1

```
class Solution:
    def transpose(self, A: List[List[int]]) -> List[List[int]]:
        return [[A[i][j] for i in range(len(A))] for j in range(len(A[0]))]
```

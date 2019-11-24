---
layout: post
title: "LeetCode题解-873(Python实现)"
description: ""
categories: [ALG]
tags: [LeetCode]
redirect_from:
  - /2019/11/24/
---



题目：

[Length of Longest Fibonacci Subsequence](https://leetcode.com/problems/length-of-longest-fibonacci-subsequence/)

解答：

```python
class Solution:
    def lenLongestFibSubseq(self, A: List[int]) -> int:
        total_set = set()
        for i in A:
            total_set.add(i)
        length = len(A)
        maxElement = A[length-1]
        maxLength = 2
        for i in range(length-2):
            for j in range(i+1, length-1):
                ii, jj, partMaxLength = A[i], A[j], 2
                while (ii+jj) <= maxElement:
                    if (ii+jj) in total_set:
                        partMaxLength  = partMaxLength + 1
                        ii, jj = jj, ii+jj
                    else:
                        break
                if partMaxLength > maxLength:
                    maxLength = partMaxLength
                # print(i, j, A[i], A[j], partMaxLength)
        if maxLength > 2:
            return maxLength
        else:
            return 0
```




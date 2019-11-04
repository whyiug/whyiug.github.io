---
layout: post
title: "LeetCode题解-3"
description: ""
categories: [ALG]
tags: [LeetCode]
redirect_from:
  - /2019/10/20/
---



题目：

[Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

解答：

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        longest = 0
        length = len(s)
        for k in range(length):
            st = set()
            long = 0
            for kk in range(k, length):
                if s[kk] in st:
                    break
                else:
                    st.add(s[kk])
                    long += 1
            if long > longest:
                longest = long
        return longest
```




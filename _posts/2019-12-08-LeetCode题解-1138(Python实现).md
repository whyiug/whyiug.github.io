---
layout: post
title: "LeetCode题解-1138(Python实现)"
description: ""
categories: [ALG]
tags: [LeetCode]
redirect_from:
  - /2019/12/08/
---



题目：

[Alphabet Board Path](https://leetcode.com/problems/alphabet-board-path/)

解答：

```python
class Solution:
    def alphabetBoardPath(self, target: str) -> str:
        alphabetMap = {}
        board = ["abcde", "fghij", "klmno", "pqrst", "uvwxy", "z"]
        for k, v in enumerate(board):
            for kk, vv in enumerate(v):
                alphabetMap[vv] = (k, kk)
        ss = ''
        for k in range(len(target)):
            if k == 0:
                ss += self.calFromStartToEnd((0, 0), alphabetMap[target[k]])
            else:
                ss += self.calFromStartToEnd(alphabetMap[target[k-1]], alphabetMap[target[k]])
        return ss

    def calFromStartToEnd(self, start, end):
        s = ''
        if start[0] - end[0] > 0:
            ud = 'U'
        else:
            ud = 'D'
        if start[1] - end[1] > 0:
            lr = 'L'
        else:
            lr = 'R'
        s, uda, lra = '', '', ''
        for v in range(abs(start[0]-end[0])):
            uda += ud
        for v in range(abs(start[1]-end[1])):
            lra += lr
        if start[0] == 5:
            s += uda + lra + '!'
        else:
            s += lra + uda + '!'
        return s



```




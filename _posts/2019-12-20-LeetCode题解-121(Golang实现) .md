---
layout: post
title: "LeetCode题解-121(Golang实现)"
description: ""
categories: [ALG]
tags: [LeetCode]
redirect_from:
  - /2019/12/20/
---



题目：

[121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

解答：

```go
func maxProfit(prices []int) int {
	var minB, maxP int
	for i := 0; i < len(prices); i++ {
		if i > 0 {
			if prices[i]-minB > maxP {
				maxP = prices[i] - minB
			}
			if prices[i] < minB {
				minB = prices[i]
			}
		} else {
			minB = prices[i]
			maxP = 0
		}
	}
	return maxP
}
```




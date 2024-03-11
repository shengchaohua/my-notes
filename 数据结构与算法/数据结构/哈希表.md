---
title: 哈希表
order: 7
category: 数据结构
tag:
  - 哈希表
  - 字典
  - Leetcode
---

<!-- more -->

## Leetcode 编程题

### 128. 最长连续序列

> [128. 最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/ "128. 最长连续序列")

一、题目

给定一个未排序的整数数组，找出最长连续序列的长度。

要求算法的时间复杂度为 O(n)。

示例 1：

```text
输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
```

示例 2：

```text
输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9
```

二、解析

使用哈希表！

代码如下：

```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        res = 0
        num_set = set(nums)

        for num in nums:
            if num - 1 not in num_set:
                cur_num = num
                cur_count = 1
                while cur_num + 1 in num_set:
                    cur_num += 1
                    cur_count += 1
                res = max(res, cur_count)
        return res
```

---
title: 队列
order: 4
category: 数据结构
tag:
  - 队列
  - Leetcode
---

<!-- more -->

## 基础

队列是一种动态集合。队列的特点是先进先出：最先进入的元素最先被删除。

1）Python

在 Python 中，可以使用列表 list 或 collections 模块下面的双端队列 deque 作为队列。

为了方便，可以使用 list，代码如下：

```python
list = []
list.append(1) # add an item to queue
list.append(2)
list.pop(0) # 1
list.pop(0) # 2
```

但是，由于出队方法 pop(0) 的复杂度为 O(N)，所以不推荐使用。

正确的做法应该使用 deque，其支持从队列两端入队和出队。代码如下：

```python
from collections import deque
queue = deque()

queue.append(1)
queue.append(2)
queue.appendleft(0)
queue.popleft() # 0
queue.pop() # 2
queue.pop() # 1
```



## Leetcode 编程题

### 225. 用队列实现栈

> [225. 用队列实现栈](https://leetcode.cn/problems/implement-stack-using-queues/)

一、题目

请你仅使用两个队列实现一个后入先出（LIFO）的栈，并支持普通栈的全部四种操作（push、top、pop 和 empty）。

实现 MyStack 类：

-   void push(int x) 将元素 x 压入栈顶。
-   int pop() 移除并返回栈顶元素。
-   int top() 返回栈顶元素。
-   boolean empty() 如果栈是空的，返回 true ；否则，返回 false 。

注意：

-   你只能使用队列的基本操作 —— 也就是 push to back、peek/pop from front、size 和 is empty 这些操作。
-   你所使用的语言也许不支持队列。 你可以使用 list （列表）或者 deque（双端队列）来模拟一个队列 , 只要是标准的队列操作即可。

二、解析

代码如下：

```python
class MyStack:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.queue = []
        self.aux_queue = []  ## 辅助队列

    def push(self, x: int) -> None:
        """
        Push element x onto stack.
        """
        self.aux_queue.append(x)
        while self.queue:
            self.aux_queue.append(self.queue.pop(0))
        self.queue, self.aux_queue = self.aux_queue, self.queue

    def pop(self) -> int:
        """
        Removes the element on top of the stack and returns that element.
        """
        return self.queue.pop(0)

    def top(self) -> int:
        """
        Get the top element.
        """
        return self.queue[0]

    def empty(self) -> bool:
        """
        Returns whether the stack is empty.
        """
        return len(self.queue) == 0
```



### 239. 滑动窗口最大值

> [239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/ "239. 滑动窗口最大值")

一、题目

给定一个数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。

返回滑动窗口中的最大值。

**示例 1：**

```纯文本
输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
输出：[3,3,5,5,6,7]
解释：
滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

示例 2：

```纯文本
输入：nums = [1], k = 1
输出：[1]
```

提示：

-   `1 <= nums.length <= 105`
-   `-104 <= nums[i] <= 104`
-   `1 <= k <= nums.length`

二、解析

滑动窗口的经典题。使用左右两个指针，右指针每次往右走，如果满足条件（或不满足条件），左指针就一直收敛。

思路：

1.  双端队列作为滑动窗口；
2.  队列中存放的是数组下标，最左边的位置存放的是当前窗口中最大元素的下标；
3.  如果当前队列大小大于等于指定大小（`i - queue[0] >= k`），则无法再添加元素，必须从左边弹出（`queue.popleft()`）；
4.  如果当前队列末尾存放的下标对应的元素小于要入队的下标对应的元素（`nums[queue[-1]] < nums[i]`），则从队尾出队（`queue.pop()`）；
5.  当前下标入队（`queue.append(i)`）。如果下标已经达到指定窗口大小，保存当前窗口的最大值。

代码如下：

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        if not nums:
            return []
        
        from collections import deque
        queue = deque()
        res = []
        for i in range(len(nums)):
            while queue and i - queue[0] >= k:
                queue.popleft()
            while queue and nums[queue[-1]] < nums[i]:
                queue.pop()
            queue.append(i)
            if i > k:
                res.append(nums[queue[0]]) 
        return res
```



### 480. 滑动窗口中位数

> [480. 滑动窗口中位数](https://leetcode-cn.com/problems/sliding-window-median/ "480. 滑动窗口中位数")

一、题目

中位数是有序序列最中间的那个数。如果序列的大小是偶数，则没有最中间的数；此时中位数是最中间的两个数的平均数。

例如：

-   \[2, 3, 4]，中位数是 3
-   \[2, 3]，中位数是 (2 + 3) / 2 = 2.5

给你一个数组 nums，有一个大小为 k 的窗口从最左端滑动到最右端。窗口中有 k 个数，每次窗口向右移动 1 位。你的任务是找出每次窗口移动后得到的新窗口中元素的中位数，并输出由它们组成的数组。

二、解析

1）二分法

```python
class Solution:
    def medianSlidingWindow(self, nums: List[int], k: int) -> List[float]:
        res = []
        window = []
        low, high = 0, 0
        while high < len(nums):
            bisect.insort_left(window, nums[high])
            while len(window) > k:
                window.pop(bisect.bisect_left(window, nums[low]))  ## 出窗
                low += 1
            if len(window) == k:
                res.append((window[k // 2] + window[(k - 1) // 2]) / 2)
            high += 1
        return res
```

2、堆

==TODO==


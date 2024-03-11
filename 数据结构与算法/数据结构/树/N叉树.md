---
title: 多叉树
order: 2
---



## 基础

### 树节点

1）Python

```python
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children  # chilren is a list
```



## Leetcode 编程题

### 589. N叉树的前序遍历

> [589. N叉树的前序遍历](https://leetcode-cn.com/problems/n-ary-tree-preorder-traversal/ "589. N叉树的前序遍历")

一、题目

给定一个 N 叉树，返回其节点值的前序遍历。

二、解析

1）递归版本

```python
class Solution:
    def preorder(self, root: 'Node') -> List[int]:
        res = []
        if root:
            res.append(root.val)
            for c in root.children:
                vals = self.preorder(c)
                res.extend(vals)

        return res
```

2）非递归版本

```python
class Solution:
    def preorder(self, root: 'Node') -> List[int]:
        if not root:
            return []

        res = []
        stack = [root]
        while stack:
            node = stack.pop()
            res.append(node.val)
            stack.extend(node.children[::-1])
        return res
```



### 590. N叉树的后序遍历

> [590. N叉树的后序遍历](https://leetcode-cn.com/problems/n-ary-tree-postorder-traversal/ "590. N叉树的后序遍历")

一、题目

给定一个 N 叉树，返回其节点值的后序遍历。

二、解析

1）递归

```python
class Solution:
    def postorder(self, root: 'Node') -> List[int]:
        res = []
        if root:
            for c in root.children:
                vals = self.postorder(c)
                res.extend(vals)
            res.append(root.val)
        
        return res
```

2）非递归

```python
class Solution:
    def postorder(self, root: 'Node') -> List[int]:
        if root is None:
            return []
        
        res = []
        stack = [root]
        while stack:
            node = stack.pop()
            res.append(node.val)
            stack.extend(node.children)
                
        return res[::-1]
```





### 429. N叉树的层序遍历

> [429. N叉树的层序遍历](https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/ "429. N叉树的层序遍历")

一、题目

给定一个 N 叉树，返回其节点值的*层序遍历*。（即从左到右，逐层遍历）。

树的序列化输入是用层序遍历，每组子节点都由 null 值分隔（参见示例）。

二、解析

代码如下：

```python
class Solution:
    def levelOrder(self, root: 'Node') -> List[List[int]]:
        if not root:
            return []
        
        queue = [root]
        res = []
        while queue:
            cur_level_vals = []
            next_level_nodes = []
            for node in queue:
                cur_level_vals.append(node.val)
                if node.children:
                    next_level_nodes.extend(node.children)
            res.append(cur_level_vals)
            queue = next_level_nodes
        return res
```



### 429-1. N叉树的层序遍历

层序遍历，返回一个列表。

代码如下：

```python
class Solution:
    def levelOrder(self, root: 'Node') -> List[List[int]]:
        if not root:
            return []
        
        from collections import deque
        res = []
        queue = deque([root])  # 双向队列deque
        while queue:
            node = queue.popleft()
            res.append(node.val)
            queue.extend(node.children)
        return res
```




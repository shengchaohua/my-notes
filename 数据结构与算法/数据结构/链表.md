---
title: 链表
order: 2
category: 数据结构
tag:
  - 链表
  - Leetcode
---

<!-- more -->

## 基础

链表是一种动态数据结构，由指针把若干个结点连接成链状结构。

在创建链表时，无须知道链表的长度。当插入一个结点时，只需要为新结点分配内存，并调整指针的指向来确保新结点被链接到链表当中。链表的内存不是在创建链表时一次性分配的，而是每添加一个结点分配一次内存，所以链表的空间效率比数组高。

但是，由于链表的内存不是连续的。如果想访问链表的结点，只能从头结点开始遍历链表，因此链表的访问时间效率低。

常见的链表有单链表，双链表。单链表中的结点有指向下一个结点的指针，而双链表既有指向上一个结点的指针，也有指向下一个结点的指针。如果一个链表的头尾结点相连，形成了一个环路，这种链表可以称为循环链表。

### 单链表结点

Python：

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
```

Golang：

```go
// Definition for singly-linked list.
type ListNode strcut {
	Val  int
	Next *ListNode
}
```

Java：

```java
public class ListNode {
    int val;
    ListNode next;
    ListNode() {}
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode next) { this.val = val; this.next = next; }
}
```



### 双链表结点

Python：

```python
## Definition for double-linked list.
class DoubleListNode:
    def __init__(self, val=0, pre=None, next=None):
        self.val = val
        self.pre = pre
        self.next = next
```

Golang：

```go
// Definition for double-linked list.
type DoubleListNode strcut {
    Val  int
    Pre  *DoubleListNode
    Next *DoubleListNode
}
```

Java:

```java
public class ListNode {
    int val;
    ListNode pre;
    ListNode next;
    ListNode() {}
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode pre, ListNode next) { this.val = val; this.pre = pre; this.next = next; }
}
```





## Leetcode 编程题

### 206. 反转链表

> [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/ "206. 反转链表")

一、题目

反转一个单链表。

二、解析

1）普通

```python
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head
        
        pre = None
        cur = head
        while cur:
            nxt = cur.next
            cur.next = pre
            pre = cur
            cur = nxt
        
        return pre
```

2）插入法

```python
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head

        dummy = ListNode(0)
        dummy.next = head
        cur = head
        while cur and cur.next:
            nxt = cur.next
            cur.next = nxt.next
            nxt.next = dummy.next
            dummy.next = nxt

        return dummy.next
```

3）递归

```python
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head
        res = self.reverseList(head.next)
        head.next.next = head
        head.next = None
        return res
```



### 92. 反转链表 II

> [92. 反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/comments/ "92. 反转链表 II")

一、题目

反转从位置 m 到 n 的链表。请使用一趟扫描完成反转。

说明: 1 ≤ m ≤ n ≤ 链表长度。

二、解析

找到第 m - 1 个结点，使用插入法。

代码如下：

```python
class Solution:
    def reverseBetween(self, head: ListNode, m: int, n: int) -> ListNode:
        if not head or m == n:
            return head
        
        dummy = ListNode(0)
        dummy.next = head
        
        pre = dummy
        for _ in range(m - 1):
            pre = pre.next
        
        cur = pre.next
        for _ in range(n - m):
            nxt = cur.next
            cur.next = nxt.next
            nxt.next = pre.next
            pre.next = nxt
        
        return dummy.next
```



### 24. 两两交换链表中的节点

> [24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/ "24. 两两交换链表中的节点")

一、题目

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。
你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

二、解析

代码如下：

```python
class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        dummy = ListNode(0)
        dummy.next = head
        
        pre = dummy
        cur = head
        while cur and cur.next:
            pre.next = cur.next
            cur.next = cur.next.next
            pre.next.next = cur
            pre = cur
            cur = cur.next
                
        return dummy.next
```



### 25. K 个一组翻转链表

> [25. K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/ "25. K 个一组翻转链表")

一、题目

给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。

k 是一个正整数，它的值小于或等于链表的长度。

如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。

二、解析

使用插入法。代码如下：

```python
class Solution:
    def reverseKGroup(self, head: ListNode, k: int) -> ListNode:
        dummy = ListNode(0)
        dummy.next = head
        
        pre = dummy
        cur = head
        while cur:
            end = cur
            for i in range(k - 1):
                end = end.next
                if not end:
                    return dummy.next
            end = end.next
            while cur and cur.next != end:
                nxt = cur.next
                cur.next = nxt.next
                nxt.next = pre.next
                pre.next = nxt
            pre = cur
            cur = end
        
        return dummy.next
```



### 234. 链表回文

> [234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/ "234. 回文链表")

一、题目

请判断一个链表是否为回文链表。

二、解析

快慢指针，并对前半部分进行翻转。

```python
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        fast = slow = head
        pre = None
        while fast and fast.next:
            fast = fast.next.next
            nxt = slow.next
            slow.next = pre
            pre = slow
            slow = nxt
        
        if fast:
            left, right = pre, slow.next
        else:    
            left, right = pre, slow
        while left and right and left.val == right.val:
            left = left.next
            right = right.next
        
        return not left and not right
```



### 143. 重排链表

> [143. 重排链表](https://leetcode-cn.com/problems/reorder-list/ "143. 重排链表")

一、题目

给定一个单链表 L：L0→L1→…→Ln-1→Ln ，

将其重新排列后变为： L0→Ln→L1→Ln-1→L2→Ln-2→…

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

二、解析

使用快慢指针，对右边的一半进行翻转。

代码如下：

```python
class Solution:
    def reorderList(self, head: ListNode) -> None:
        """
        Do not return anything, modify head in-place instead.
        """
        if not head or not head.next:
            return
        ## 快慢指针，确定重点
        slow, fast = head, head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        right = slow.next
        slow.next = None
        ## 右半部分链表反转
        pre, cur = None, right 
        while cur:
            temp = cur.next 
            cur.next = pre
            pre = cur
            cur = temp
        ## 拼接两个链表
        left, right = head, pre
        while left and right:
            temp2 = left.next
            temp3 = right.next
            left.next = right
            right.next = temp2
            left = temp2
            right = temp3
```



### 61. 旋转链表

> [61. 旋转链表](https://leetcode-cn.com/problems/rotate-list/ "61. 旋转链表")

一、题目

给定一个链表，旋转链表，将链表每个节点向右移动 k 个位置，其中 k 是非负数。

二、解析

> 参考 [Leetcode-Gallianoo](https://leetcode-cn.com/u/gallianoo/ "Leetcode-Gallianoo")

遍历求链表总长度，将链表首尾相连。

代码如下：

```python
class Solution:
    def rotateRight(self, head: ListNode, k: int) -> ListNode:
        if not head or not head.next or k == 0:
            return head
        
        count = 1
        cur = head
        while cur and cur.next:
            count += 1
            cur = cur.next
        
        k %= count
        if k == 0:
            return head
        
        cur.next = head  ## 首尾相连
        for i in range(count - k):
            cur = cur.next

        nxt = cur.next
        cur.next = None
        return nxt
```



### 21. 合并两个有序链表

> [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/ "21. 合并两个有序链表")

一、题目

将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。

二、解析

1）循环

```python
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        dummy = ListNode(None)
        p = dummy

        while l1 and l2:
            if l1.val < l2.val:
                p.next = l1
                l1 = l1.next
                p = p.next
            else:
                p.next = l2
                l2 = l2.next
                p = p.next
        
        p.next = l1 if l1 else l2
        return dummy.next
```

2）递归

```python
class Solution:
    def mergeTwoLists(self, l1, l2):
        if l1 is None:
            return l2
        elif l2 is None:
            return l1
        elif l1.val < l2.val:
            l1.next = self.mergeTwoLists(l1.next, l2)
            return l1
        l2.next = self.mergeTwoLists(l1, l2.next)
        return l2
```



### 23. 合并K个有序链表

> [https://leetcode.cn/problems/merge-k-sorted-lists/](https://leetcode.cn/problems/merge-k-sorted-lists/ "https://leetcode.cn/problems/merge-k-sorted-lists/")

一、题目

给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

示例 1：

```text
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
```

二、解析

> 参考 [LeetCode-Solution](https://leetcode-cn.com/problems/merge-k-sorted-lists/solution/he-bing-kge-pai-xu-lian-biao-by-leetcode-solutio-2/ "LeetCode-Solution")

1）一个一个合并。

```python
class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        def merge_two_sorted_list(l1, l2):
            fake = ListNode(None)
            cur = fake

            while l1 and l2:
                if l1.val < l2.val:
                    cur.next = l1
                    l1 = l1.next
                    cur = cur.next
                else:
                    cur.next = l2
                    l2 = l2.next
                    cur = cur.next
            
            cur.next = l1 if l1 else l2
            return fake.next

        ans = None
        for l in lists:
            ans = merge_two_sorted_list(ans, l)

        return ans
```

分析：

-   时间复杂度：假设每个链表的最长长度是 $n$。在第一次合并后，ans的长度为 $n$；第二次合并后，ans的长度为 $2\times n$。第 $i$ 次合并后，ans的长度为 $i \times n$。第 $i$ 次合并的时间代价是 $O(n + (i - 1) \times n) = O(i \times n)$，那么总的时间代价为 $O(\sum_{i = 1}^{k} (i \times n)) = O(\frac{(1 + k)\cdot k}{2} \times n)$，故渐进时间复杂度为 $O(k^2 n)$。
-   空间复杂度：没有用到与 $k$ 和 $n$ 规模相关的辅助空间，故渐进空间复杂度为 $O(1)$。

2）分治法。虽然也是两个两个合并，但是使用分治的方法。可以参考归并排序。

代码如下：

```python
class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        def merge_two_sorted_list(l1, l2):
            ## 与 方法1）相同
            pass

        def helper(lists):
            if len(lists) == 0:
                return None
            if len(lists) == 1:
                return lists[0]
            
            mid = len(lists) // 2
            left = helper(lists[:mid])
            right = helper(lists[mid:])
            return merge_two_sorted_list(left, right)

        return helper(lists)
```

分析：

-   时间复杂度：考虑递归「向上回升」的过程——第一轮合并 $ \frac{k}{2}  $ 组链表，每一组的时间代价是 $O(2n)$；第二轮合并 $ \frac{k}{4}  $组链表，每一组的时间代价是 $O(4n)$......所以总的时间代价是 $O(\sum_{i = 1}^{\infty} \frac{k}{2^i} \times 2^i n)$，故渐进时间复杂度为 $O(kn \times \log k)$。
-   空间复杂度：递归会使用到 $O(\log k)$ 空间代价的栈空间。

3）使用优先队列。优先队列用来维护每个链表的最小的结点。

代码如下：

```java
class Solution {
    class Status implements Comparable<Status> {
        int val;
        ListNode ptr;

        Status(int val, ListNode ptr) {
            this.val = val;
            this.ptr = ptr;
        }

        public int compareTo(Status status2) {
            return this.val - status2.val;
        }
    }

    PriorityQueue<Status> queue = new PriorityQueue<Status>();

    public ListNode mergeKLists(ListNode[] lists) {
        for (ListNode node: lists) {
            if (node != null) {
                queue.offer(new Status(node.val, node));
            }
        }
        ListNode head = new ListNode(0);
        ListNode tail = head;
        while (!queue.isEmpty()) {
            Status f = queue.poll();
            tail.next = f.ptr;
            tail = tail.next;
            if (f.ptr.next != null) {
                queue.offer(new Status(f.ptr.next.val, f.ptr.next));
            }
        }
        return head.next;
    }
}

```



### 147. 对链表进行插入排序

> [147. 对链表进行插入排序](https://leetcode-cn.com/problems/insertion-sort-list/ "147. 对链表进行插入排序")

一、题目

给定单个链表的头 head ，使用 插入排序 对链表进行排序，并返回 排序后链表的头 。

插入排序 算法的步骤:

1. 插入排序是迭代的，每次只移动一个元素，直到所有元素可以形成一个有序的输出列表。
2. 每次迭代中，插入排序只从输入数据中移除一个待排序的元素，找到它在序列中适当的位置，并将其插入。
3. 重复直到所有输入数据插入完为止。

下面是插入排序算法的一个图形示例。部分排序的列表(黑色)最初只包含列表中的第一个元素。每次迭代时，从输入数据中删除一个元

素(红色)，并就地插入已排序的列表中。

![img](https://raw.githubusercontent.com/shengchaohua/my-images/main/images/202312301015470.gif)

二、解析

使用 next 指针比较方便，不需要保存当前结点的前一个结点。

代码如下：

```python
class Solution:
    def insertionSortList(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head
        
        dummy = ListNode(None)
        dummy.next = head

        cur = head
        while cur and cur.next:
            if cur.val <= cur.next.val:
                cur = cur.next
                continue
            
            pre = dummy
            while pre.next.val <= cur.next.val:
                pre = pre.next
            ## 插入法，把nxt插在pre后面
            nxt = cur.next
            cur.next = nxt.next
            nxt.next = pre.next
            pre.next = nxt

        return dummy.next
```



### 148. 排序链表 - O(nlgn)

> [148. 排序链表](https://leetcode-cn.com/problems/sort-list/ "148. 排序链表")

一、题目

给你链表的头结点 `head` ，请将其按 **升序** 排列并返回 **排序后的链表** 。

**进阶：** 你可以在`O(nlogn)` 时间复杂度和常数级空间复杂度下，对链表进行排序吗？

二、解析

1）使用归并排序。

```python
class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        def merge_two_sorted_list(l1, l2):
            dummy = ListNode(None)
            cur = dummy
            while l1 and l2:
                if l1.val < l2.val:
                    cur.next = l1
                    l1 = l1.next
                    cur = cur.next
                else:
                    cur.next = l2
                    l2 = l2.next
                    cur = cur.next
            cur.next = l1 if l1 else l2
            return dummy.next

        def merge_sort(head):
            if not head or not head.next:
                return head
            pre = None
            fast = slow = head
            while fast and fast.next:
                pre = slow
                slow = slow.next
                fast = fast.next.next
            pre.next = None

            left = merge_sort(head)
            right = merge_sort(slow)
            return merge_two_sorted_list(left, right)
        
        return merge_sort(head)
```

2）使用快速排序。

> 参考 [Leetcode-a380922457](https://leetcode-cn.com/problems/sort-list/solution/gui-bing-pai-xu-he-kuai-su-pai-xu-by-a380922457/ "Leetcode-a380922457")

快速排序的第三种方法，选择头结点作为轴元素，因为选尾结点需要遍历一遍链表。

Python实现。代码没什么问题，但是最后一个测试用例没通过，结果超时。

```python
class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        def partition(head, tail):
            pivot = head.val
            store_node = head
            cur = head.next
            while cur != tail:
                if cur.val <= pivot:
                    store_node = store_node.next
                    store_node.val, cur.val = cur.val, store_node.val
                cur = cur.next
            head.val, store_node.val = store_node.val, head.val
            return store_node

        def quick_sort(head, tail):
            if head == tail or head.next == tail:
                return
            par_node = partition(head, tail)
            quick_sort(head, par_node)
            quick_sort(par_node.next, tail)
            
        quick_sort(head, None)
        return head
```

Java实现，逻辑相同，通过！

```java
class Solution {
    public ListNode sortList(ListNode head) {
        quickSort(head, null);
        return head;
    }

    public void quickSort(ListNode head, ListNode tail) {
        if (head == tail || head.next == tail)
            return;
        ListNode par_node = partition(head, tail);
        quickSort(head, par_node);
        quickSort(par_node.next, tail);
    }

    public ListNode partition(ListNode head, ListNode tail) {
        int pivot = head.val;
        ListNode store_node = head;
        ListNode cur = head.next;
        while (cur != tail) {
            if (cur.val <= pivot) {
                store_node = store_node.next;
                swap(store_node, cur);
            }
            cur = cur.next;
        }
        swap(head, store_node);
        return store_node;
    }
    
    public void swap(ListNode n1, ListNode n2) {
        int temp = n1.val;
        n1.val = n2.val;
        n2.val = temp;
    }
}
```



### 2. 两数相加

> [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/ "2. 两数相加")

一、题目

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 **逆序** 的方式存储的，并且它们的每个节点只能存储 一位 数字。
如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。
您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

二、解析

```python
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        dummy = ListNode(0)
        cur = dummy
        
        p1, p2 = l1, l2
        carry = 0
        while p1 or p2 or carry != 0:
            x = p1.val if p1 else 0
            y = p2.val if p2 else 0
            carry, remainder = divmod(x + y + carry, 10)
            cur.next = ListNode(remainder)
            cur = cur.next
            if p1: p1 = p1.next
            if p2: p2 = p2.next

        return dummy.next
```



### 445. 两数相加 II

> [445. 两数相加 II](https://leetcode-cn.com/problems/add-two-numbers-ii/ "445. 两数相加 II")

一、题目

给你两个 非空 链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。
你可以假设除了数字 0 之外，这两个数字都不会以零开头。
进阶：如果输入链表不能修改该如何处理？换句话说，你不能对列表中的节点进行翻转。

二、解析

使用栈！代码如下：

```python
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        s1, s2 = [], []
        while l1:
            s1.append(l1.val)
            l1 = l1.next
        while l2:
            s2.append(l2.val)
            l2 = l2.next
        ans = None
        carry = 0
        while s1 or s2 or carry != 0:
            a = 0 if not s1 else s1.pop()
            b = 0 if not s2 else s2.pop()
            cur = a + b + carry
            carry = cur // 10
            cur %= 10
            curnode = ListNode(cur)
            curnode.next = ans
            ans = curnode
        return ans
```



### 160. 相交链表

> [160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists)

一、题目

给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 null 。

题目数据 **保证** 整个链式结构中不存在环。

**注意**，函数返回结果后，链表必须 **保持其原始结构** 。

二、解析

假装两个链表连接在一起，遍历完一个链表再遍历另一个链表。

代码如下：

```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        if not headA or not headB:
            return None
        
        pA = headA
        pB = headB
        while pA != pB:
            pA = pA.next if pA else headB
            pB = pB.next if pB else headA
        return pA
```



### 141. 环形链表

> [141. 环形链表](https://leetcode.cn/problems/linked-list-cycle/)

一、题目

给你一个链表的头节点 head ，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。注意：pos 不作为参数进行传递 。仅仅是为了标识链表的实际情况。

如果链表中存在环 ，则返回 true 。 否则，返回 false 。

二、解析

> [https://leetcode.cn/problems/find-the-duplicate-number/solution/xun-zhao-zhong-fu-shu-by-leetcode-solution/](https://leetcode.cn/problems/find-the-duplicate-number/solution/xun-zhao-zhong-fu-shu-by-leetcode-solution/ "https://leetcode.cn/problems/find-the-duplicate-number/solution/xun-zhao-zhong-fu-shu-by-leetcode-solution/")

1）哈希表。

代码如下：

```python
class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        seen = set()
        while head:
            if head in seen:
                return True
            seen.add(head)
            head = head.next
        return False

```

2）快慢指针。快指针一次走两步，慢指针一次走一步，如果链表有环，那么两个指针一定会相遇。

代码如下：

```python
class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        if not head or not head.next:
            return False
        
        slow = head
        fast = head.next

        while slow != fast:
            if not fast or not fast.next:
                return False
            slow = slow.next
            fast = fast.next.next
        
        return True

```



### 142. 环形链表II

> [https://leetcode.cn/problems/linked-list-cycle-ii/](https://leetcode.cn/problems/linked-list-cycle-ii/ "https://leetcode.cn/problems/linked-list-cycle-ii/")

一、题目

给定一个链表的头节点  head ，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

不允许修改链表。

二、解析

1）使用哈希表。

Golang代码如下：

```go
func detectCycle(head *ListNode) *ListNode {
    seen := map[*ListNode]struct{}{}
    for head != nil {
        if _, ok := seen[head]; ok {
            return head
        }
        seen[head] = struct{}{}
        head = head.Next
    }
    return nil
}
```

2）快慢指针。快慢指针相遇时，再定义一个指针从头开始走，新指针和慢指针会在链表开始入环的第一个节点相遇。

Golang代码如下：

```go
func detectCycle(head *ListNode) *ListNode {
    slow, fast := head, head
    for fast != nil {
        slow = slow.Next
        if fast.Next == nil {
            return nil
        }
        fast = fast.Next.Next
        if fast == slow {
            p := head
            for p != slow {
                p = p.Next
                slow = slow.Next
            }
            return p
        }
    }
    return nil
}

```



### 138. 复制带随机指针的链表

> [https://leetcode.cn/problems/copy-list-with-random-pointer/](https://leetcode.cn/problems/copy-list-with-random-pointer/ "https://leetcode.cn/problems/copy-list-with-random-pointer/")

一、题目

给你一个长度为 n 的链表，每个节点包含一个额外增加的随机指针 random ，该指针可以指向链表中的任何节点或空节点。

构造这个链表的 深拷贝。 深拷贝应该正好由 n 个 全新 节点组成，其中每个新节点的值都设为其对应的原节点的值。新节点的 next 指针和 random 指针也都应指向复制链表中的新节点，并使原链表和复制链表中的这些指针能够表示相同的链表状态。复制链表中的指针都不应指向原链表中的节点 。

例如，如果原链表中有 X 和 Y 两个节点，其中 X.random --> Y 。那么在复制链表中对应的两个节点 x 和 y ，同样有 x.random --> y 。

返回复制链表的头节点。

用一个由 n 个节点组成的链表来表示输入/输出中的链表。每个节点用一个 \[val, random\_index] 表示：

-   val：一个表示 Node.val 的整数。
-   random\_index：随机指针指向的节点索引（范围从 0 到 n-1）；如果不指向任何节点，则为  null 。
    你的代码 只 接受原链表的头节点 head 作为传入参数。

提示：

-   0 <= n <= 1000
-   -104 <= Node.val <= 104
-   Node.random 为 null 或指向链表中的节点。

二、解析

代码如下：

```python
class Solution:
    def copyRandomList(self, head: 'Node') -> 'Node':
        if not head:
            return None
        
        p = head
        seen = {}
        while p:
            seen[p] = Node(p.val)
            p = p.next
        
        fake = cur = Node(0)
        p = head
        while p:
            tmp = seen[p]
            tmp.next = seen[p.next] if p.next in seen else None
            tmp.random = seen[p.random] if p.random in seen else None
            cur.next = tmp
            cur = cur.next
            p = p.next
        
        return fake.next
```

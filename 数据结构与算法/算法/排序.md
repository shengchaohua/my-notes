---
title: 排序
order: 1
---





## 基础

### 冒泡排序

```python
def bubble_sort(A):
    for i in range(len(A)):
        for j in range(0, len(A) - i - 1):
            if A[j] > A[j + 1]:
                A[j], A[j + 1] = A[j + 1], A[j]
```

### 插入排序

```python
def insert_sort(A):
    for i in range(1, len(A)):
        temp = A[i]
        j = i - 1
        while j >= 0 and A[j] > temp:
            A[j + 1] = A[j]
            j -= 1
        A[j + 1] = temp  # 不满条件的下一个位置
```

### 选择排序

```python
def select_sort(A):
    for i in range(0, len(A)):
        k = i
        # Find the smallest num and record its index
        for j in range(i, len(A)):
            if A[j] < A[k]:
                k = j
        if k != i:
            A[i], A[k] = A[k], A[i]
```

### 归并排序

1）返回数组法。

代码如下：

```python
def merge(A, B):
    # return sorted(A + B)
    temp = []
    i, j = 0, 0
    while i < len(A) and j < len(B):
        if A[i] <= B[j]:
            temp.append(A[i])
            i += 1
        else:
            temp.append(B[j])
            j += 1
    temp.extend(A[i:])
    temp.extend(B[j:])
    return temp


def merge_sort(A):
    if len(A) <= 1:
        return A
    mid = len(A) // 2
    left = merge_sort(A[:mid])
    right = merge_sort(A[mid:])
    return merge(left, right)
    
# res = merge_sort(A)
```

2）原地操作法。函数参数稍微复杂。

```python
def merge(A, left, mid, right):
    """A[left:mid+1], A[mid+1:right+1]"""
    temp = []  # 用于存储数字
    i, j = left, mid + 1
    while i <= mid and j <= right:
        if A[i] <= A[j]:
            temp.append(A[i])
            i += 1
        else:
            temp.append(A[j])
            j += 1
    temp.extend(A[i: mid + 1])
    temp.extend(A[j: right + 1])
    A[left: right + 1] = temp


def merge_sort(A, left, right):
    if left < right:
        mid = (left + right) // 2
        merge_sort(A, left, mid)
        merge_sort(A, mid + 1, right)
        merge(A, left, mid, right)
        
# merge_sort(A, 0, len(A) - 1)
```

### 快速排序

介绍三种写法的快速排序。三种写法的quick_sort函数相同，在此给出。

推荐使用**填坑法**，比较直观，便于记忆。

```python
def quick_sort(A, left, right):
    if left < right:
        p = partition(A, left, right)
        quick_sort(A, left, p - 1)
        quick_sort(A, p + 1, right)
        
# quick_sort(A, 0, len(A) - 1)
```

1）填坑法。

使用第一个元素当作轴元素。注意先从右往左比较，大于等于号；再从左往右比较，小于号。

```python
def partition(A, left, right):
    pivot = A[left]
    while left < right:
        while left < right and A[right] >= pivot:
            right -= 1
        A[left] = A[right]  # 看作填坑
        while left < right and A[left] < pivot:
            left += 1
        A[right] = A[left]  # 看作填坑
    A[left] = pivot
    return left
```

2）交换法。

使用第一个元素当作轴元素。注意先从右往左比较，大于等于号；再从左往右比较，小于等于号。

```python
def partition(A, left, right):
    pivot_idx = left
    pivot = A[left]
    while left < right:
        while left < right and A[right] >= pivot:
            right -= 1
        while left < right and A[left] <= pivot:
            left += 1
        if left < right:  # 进行交换
            A[left], A[right] = A[right], A[left]
    A[pivot_idx], A[left] = A[left], A[pivot_idx]
    return left
```

3）顺序遍历法。

算法导论中的写法，选择最后一个元素作为轴元素。

```python
def partition(A, left, right):
    pivot = A[right]  # 选择最后一个元素作为轴元素
    store_idx = left - 1
    for cur in range(left, right):  # 顺序遍历
        if A[cur] <= pivot:
            store_idx += 1
            A[store_idx], A[cur] = A[cur], A[store_idx]
    A[store_idx + 1], A[right] = A[right], A[store_idx + 1]
    return store_idx + 1
```

选择第一个元素作为轴元素，而且该写法可以推广到链表排序。

```python
def partition(A, left, right):
    pivot = A[left]  # 选择第一个元素作为轴元素
    store_idx = left
    for cur in range(left + 1, right + 1):  # 顺序遍历
        if A[cur] <= pivot:
            store_idx += 1
            A[store_idx], A[cur] = A[cur], A[store_idx]
    A[left], A[store_idx] = A[store_idx], A[left]
    return store_idx
```

### 堆排序

> [Python实现 《算法导论 第三版》中的算法 第6章 堆排序](https://blog.csdn.net/shengchaohua163/article/details/83038413 "Python实现 《算法导论 第三版》中的算法 第6章 堆排序")

下面的代码实现了一个最大堆以及堆排序算法：

```python
def get_parent(i):
    return (i - 1) // 2


def get_left(i):
    return 2 * i + 1


def get_right(i):
    return 2 * i + 2


def max_heapify_recursive(A, heap_size, i):
    l = get_left(i)
    r = get_right(i)
    largest_ind = i
    if l < heap_size and A[l] > A[largest_ind]:
        largest_ind = l
    if r < heap_size and A[r] > A[largest_ind]:
        largest_ind = r
    if largest_ind == i:
        return
    else:
        A[i], A[largest_ind] = A[largest_ind], A[i]
        max_heapify_recursive(A, heap_size, largest_ind)
    
    
def max_heapify_loop(A, heap_size, i): 
    while i < heap_size:
        l = get_left(i)
        r = get_right(i)
        largest_ind = i
        if l < heap_size and A[l] > A[largest_ind]:
            largest_ind = l
        if r < heap_size and A[r] > A[largest_ind]:
            largest_ind = r
        if largest_ind == i:
            break
        else:
            A[i], A[largest_ind] = A[largest_ind], A[i]
            i = largest_ind


def build_max_heap(A, heap_size):
    begin = len(A)//2 - 1  # len(A)//2 - 1是堆中第一个叶子节点的前一个节点
    for i in range(begin, -1, -1):
        max_heapify_loop(A, heap_size, i)


def heap_sort(A):
    heap_size = len(A)
    build_max_heap(A, heap_size)
    for i in range(len(A)-1, 0, -1):
        A[0], A[i] = A[i], A[0]  # 每次固定最后一个元素，并将堆大小减一
        heap_size -= 1
        max_heapify_loop(A, heap_size, 0)
```



### 线性时间排序

> [Python实现 《算法导论 第三版》中的算法 第8章 线性时间排序](https://blog.csdn.net/shengchaohua163/article/details/83444059 "Python实现 《算法导论 第三版》中的算法 第8章 线性时间排序")

上面的几种算法都是基于比较的算法，时间复杂度最好可以达到$O(n\lg n)$，比如归并排序、堆排序和快速排序。归并排序和堆排序在最坏情况下能够达到该复杂度，快速排序在平均情况达到该复杂度。注意，快速排序最坏情况下是$O(n^2)$。

下面介绍一下三种线性时间复杂度的排序算法：计数排序、基数排序和桶排序。




## Leetcode 编程题
### LCR 164. 把数组排成最小的数

> [LCR 164. 破解闯关密码](https://leetcode.cn/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)

一、题目

输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。

二、解析

字典顺序。

代码如下：

```python
class Solution:
    def minNumber(self, nums: List[int]) -> str:
        str_nums = [str(num) for num in nums]  
        for i in range(len(nums)):
            for j in range(len(nums) - i - 1):
                if str_nums[j] + str_nums[j + 1] > str_nums[j + 1] + str_nums[i]:
                    str_nums[j], str_nums[j + 1] = str_nums[j + 1], str_nums[j]
        
        return "".join(str_nums)
```


### 56. 合并区间

> [56. 合并区间](https://leetcode.cn/problems/merge-intervals/)

一、题目

给出一个区间的集合，请合并所有重叠的区间。

二、解析

排序即可。

代码如下：

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals.sort(key=lambda ele: ele[0])
        res = []
        for inter in intervals:
            if res and res[-1][1] >= inter[0]:
                last = res.pop()
                res.append([last[0], max(last[1], inter[1])])
            else:
                res.append(inter)
        return res
```



### 406. 根据身高重建队列

> [406. 根据身高重建队列](https://leetcode.cn/problems/queue-reconstruction-by-height/)

一、题目

假设有打乱顺序的一群人站成一个队列。 每个人由一个整数对(h, k)表示，其中h是这个人的身高，k是排在这个人前面且身高大于或等于h的人数。 编写一个算法来重建这个队列。
注意：总人数少于1100人。

二、解析

按身高降序、人数升序排序。

> 参考 [Leetcode 官方题解](https://leetcode-cn.com/problems/queue-reconstruction-by-height/solution/gen-ju-shen-gao-zhong-jian-dui-lie-by-leetcode/ "Leetcode 官方题解")

代码如下：

```python
class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        people.sort(key = lambda x: (-x[0], x[1]))
        res = []
        for p in people:
            res.insert(p[1], p)
        return res
```



### LCR 170. 交易逆序对的总数

> [LCR 170. 交易逆序对的总数](https://leetcode.cn/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

一、题目

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

二、解析

使用归并排序。

代码如下：

```python
class Solution:
    def reversePairs(self, nums: List[int]) -> int:
        def merge(A, B):
            temp = []
            i, j = 0, 0
            while i < len(A) and j < len(B):
                if A[i] <= B[j]:
                    temp.append(A[i])
                    i += 1
                else:
                    temp.append(B[j])
                    j += 1
                    self.res += len(A) - i
            temp.extend(A[i:])
            temp.extend(B[j:])
            return temp

        def merge_sort(A):
            if len(A) <= 1:
                return A
            mid = len(A) // 2
            left = merge_sort(A[:mid])
            right = merge_sort(A[mid:])
            return merge(left, right)

        self.res = 0
        merge_sort(nums)
        return self.res
```



### 493. 翻转对

> [493. 翻转对](https://leetcode-cn.com/problems/reverse-pairs/)

一、题目

给定一个数组nums，如果i < j且nums\[i] > 2 \* nums\[j]我们就将(i, j)称作一个重要翻转对。你需要返回给定数组中的重要翻转对的数量。

二、解析

代码如下：

```python
class Solution:
    def reversePairs(self, nums: List[int]) -> int:
        def merge(A, left, mid, right):
            """A[left:mid+1], A[mid+1:right+1]"""
            temp = []  # 用于存储数字
            i, j = left, mid + 1
            while i <= mid and j <= right:
                if A[i] <= A[j]:
                    temp.append(A[i])
                    i += 1
                else:
                    temp.append(A[j])
                    j += 1
            temp.extend(A[i: mid + 1])
            temp.extend(A[j: right + 1])
            A[left: right + 1] = temp


        def merge_sort(A, left, right):
            if left >= right:
                return 0
            mid = (left + right) // 2
            count = merge_sort(A, left, mid) + merge_sort(A, mid + 1, right)
            j = mid + 1
            for i in range(left, mid + 1):
                while j <= right and A[i] > A[j] * 2:
                    j += 1
                count += j - mid - 1
            merge(A, left, mid, right)
            return count
        
        return merge_sort(nums, 0, len(nums) - 1)
```



### 315. 计算右侧小于当前元素的个数

> [315. 计算右侧小于当前元素的个数](https://leetcode.cn/problems/count-of-smaller-numbers-after-self/)

一、题目

给定一个整数数组 nums，按要求返回一个新数组counts。数组counts有该性质：counts\[i] 的值是nums\[i]右侧小于nums\[i]的元素的数量。

二、解析

二分查找、归并排序、树状数组等。

1）二分查找

```python
class Solution:
    def countSmaller(self, nums: List[int]) -> List[int]:
        import bisect
        queue = []
        res = []
        for num in nums[::-1]:
            loc = bisect.bisect_left(queue, num)
            res.append(loc)
            queue.insert(loc, num)
        return res[::-1]
```

2）归并排序

```python
class Solution:
    def countSmaller(self, nums: List[int]) -> List[int]:
        def merge(left, right):
            temp = []
            i = j = 0
            while i < len(left) or j < len(right):
                if j == len(right) or i < len(left) and left[i][1] <= right[j][1]:
                    temp.append(left[i])
                    res[left[i][0]] += j
                    i += 1
                else:
                    temp.append(right[j])
                    j += 1
            return temp
        
        def merge_sort(nums):
            if len(nums) <= 1:
                return nums
            mid = len(nums) // 2
            left = merge_sort(nums[:mid])
            right = merge_sort(nums[mid:])
            return merge(left, right)
        
        res = [0] * len(nums)
        arr = [[i, num] for i, num in enumerate(nums)]
        merge_sort(arr)
        return res
```

3）树状数组

TODO



### 327. 区间和的个数

> [327. 区间和的个数](https://leetcode.cn/problems/count-of-range-sum/)

一、题目

给定一个整数数组nums，返回区间和在\[lower,upper]之间的个数，包含lower和upper。区间和S(i, j)表示在nums中，位置从i到j的元素之和，包含i和j(i≤j)。说明: 最直观的算法复杂度是O(n^2)，请在此基础上优化你的算法。

二、解析

TODO

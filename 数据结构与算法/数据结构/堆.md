---
title: 堆
order: 5
category: 数据结构
tag:
  - 堆
  - Leetcode
---

<!-- more -->

## 基础

### 堆实现和堆排序

> [Python实现 《算法导论 第三版》中的算法 第6章 堆排序](https://blog.csdn.net/shengchaohua163/article/details/83038413 "Python实现 《算法导论 第三版》中的算法 第6章 堆排序")

下面使用 Python 实现了一个最大堆以及堆排序：

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
    begin = len(A)//2 - 1  ## len(A)//2 - 1是堆中第一个叶子节点的前一个节点
    for i in range(begin, -1, -1):
        # max_heapify_recusive(A, heap_size, i)
        max_heapify_loop(A, heap_size, i)


def heap_sort(A):
    heap_size = len(A)
    build_max_heap(A, heap_size)
    for i in range(len(A)-1, 0, -1):
        A[0], A[i] = A[i], A[0]  ## 每次固定最后一个元素，并将堆大小减一
        heap_size -= 1
        max_heapify_loop(A, heap_size, 0)
```



### 不同编程语言中的实现

1）Python

在 Python 中，内置的 heapq 模块提供了最小堆的实现，如下所示：

```python
import heapq

nums = [0, 1, 5, 3, 2]     # creates an empty heap
heapq.heapify(nums)        # transforms list into a heap, in-place, in linear time

item = heap[0]             # smallest item on the heap without popping it
heapq.heappush(nums, 4)    # pushes a new item on the heap
item = heapq.heappop(nums) # pops the smallest item from the heap

heapq.heappushpop(nums, 4)
item = heapq.heapreplace(heap, item) # pops and returns smallest item, and adds new item; the heap size is unchanged
```

2）Java

在 Java 中，可以使用 ProrityQueue 可以用来实现堆。

```java
import java.util.Comparator;
import java.util.PriorityQueue;

public class Heap {
    public static void main(String[] args) {
        PriorityQueue<Integer> queue = new PriorityQueue<Integer>(new Comparator<Integer>() {
            public int compare(Integer m, Integer n) {
                return m - n;
            }
        });

        queue.add(5);
        queue.add(1);
        queue.add(0);
        queue.add(3);
        queue.add(2);
        System.out.println(queue.stream().toList());
    }
}
```

3）Golang

在 Golang 中，"container/heap"包实现了通用的最小堆，并提供了接口，便于实现自定义的堆。

堆接口如下所示：

```go
type Interface interface {
	sort.Interface
	Push(x any) // add x as element Len()
	Pop() any   // remove and return element Len() - 1.
}

// sort.Interface
type Interface interface {
	Len() int
	Less(i, j int) bool
	Swap(i, j int)
}
```

根据上面的代码，自定义的堆需要实现五个方法。下面给出一个最简单的实现：

```go
import (
	"container/heap"
	"fmt"
)

type MyHeap []int

func (h MyHeap) Len() int {
	return len(h)
}

func (h MyHeap) Less(i, j int) bool {
	return h[i] < h[j]
}

func (h MyHeap) Swap(i, j int) {
	h[i], h[j] = h[j], h[i]
	return
}

func (h *MyHeap) Push(x any) {
	*h = append(*h, x.(int))
}

func (h *MyHeap) Pop() any {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}

func main() {
	h := &MyHeap{4, 3, 2, 1, 0}
	heap.Init(h)
	fmt.Println(*h) // [0 1 2 4 3]
}
```



## Leetcode 编程题

### 347. 前K个高频元素

> [347. 前K个高频元素](https://leetcode.cn/problems/top-k-frequent-elements)

一、题目

给你一个整数数组 nums 和一个整数 k ，请你返回其中出现频率前 k 高的元素。你可以按 任意顺序 返回答案。

示例 1:

```python
输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]
```

示例 2:

```text
输入: nums = [1], k = 1
输出: [1]
```

提示：

-   1 <= nums.length <= 105
-   k 的取值范围是 \[1, 数组中不相同的元素的个数]
-   题目数据保证答案唯一，换句话说，数组中前 k 个高频元素的集合是唯一的&#x20;

进阶：你所设计算法的时间复杂度 必须 优于 O(n log n) ，其中 n 是数组大小。

二、解析

使用一个最小堆，用频率作为判断。

代码如下：

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        num_freq = {}
        for num in nums:
            if num not in num_freq:
                num_freq[num] = 0
            num_freq[num] += 1
        l = []
        for num, freq in num_freq.items():
            l.append((freq, num))
        
        import heapq
        heap = []
        for ele in l:
            if len(heap) < k:
                heapq.heappush(heap, ele)
            else:
                if heap[0][0] < ele[0]:
                    heapq.heappushpop(heap, ele)
        
        res = []
        while heap:
            res.append(heap.pop()[1])
        return res
```

Golang 代码如下：

```go
func topKFrequent(nums []int, k int) []int {
    occurrences := map[int]int{}
    for _, num := range nums {
        occurrences[num]++
    }
    h := &IHeap{}
    heap.Init(h)
    for key, value := range occurrences {
        heap.Push(h, [2]int{key, value})
        if h.Len() > k {
            heap.Pop(h)
        }
    }
    ret := make([]int, k)
    for i := 0; i < k; i++ {
        ret[k - i - 1] = heap.Pop(h).([2]int)[0]
    }
    return ret
}

type IHeap [][2]int

func (h IHeap) Len() int           { return len(h) }
func (h IHeap) Less(i, j int) bool { return h[i][1] < h[j][1] }
func (h IHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *IHeap) Push(x interface{}) {
    *h = append(*h, x.([2]int))
}

func (h *IHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}
```


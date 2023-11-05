[TOC]







# 前言

刷leetcode时随手记录！默认使用Python语言！



# 数组

## 两数之和

> [Leetcode 1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

> 给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那两个整数，并返回他们的数组下标。
>
> 你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

**解析**：
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        saved = {}
        for i, num in enumerate(nums):
            if target - num in saved:
                return [saved[target-num], i]
            else:
                saved[num] = i 
```

## 两数之和 II - 输入有序数组

> [Leetcode 167. 两数之和 II - 输入有序数组](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)

> 给定一个已按照升序排列 的有序数组，找到两个数使得它们相加之和等于目标数。
>
> 函数应该返回这两个下标值 index1 和 index2，其中 index1 必须小于 index2。

**解析**：双指针。

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left = 0
        right = len(numbers) - 1
        while left < right:
            if numbers[left] + numbers[right] == target:
                return [left + 1, right + 1]
            elif numbers[left] + numbers[right] < target:
                left += 1
            else:
                right -= 1
        
        return []
```

## 三数之和

> [Leetcode 15. 三数之和](https://leetcode-cn.com/problems/3sum/)

> 给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。
>
> 注意：答案中不可以包含重复的三元组。

**解析**：注意去重！

> 参考 [Leetcode-guanpengchn](https://leetcode-cn.com/problems/3sum/solution/hua-jie-suan-fa-15-san-shu-zhi-he-by-guanpengchn/)

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        if not nums or len(nums) < 3:
            return []
        
        res = []
        nums.sort()
        for i in range(len(nums) - 2):
            if nums[i] > 0:
                break
            if i > 0 and nums[i] == nums[i - 1]:
                continue  
            left = i + 1
            right = len(nums) - 1
            while left < right:
                temp = nums[i] + nums[left] + nums[right]
                if temp == 0:
                    res.append([nums[i], nums[left], nums[right]])
                    while left < right and nums[left] == nums[left + 1]: 
                        left += 1
                    while left < right and nums[right] == nums[right - 1]:
                        right -= 1
                    left += 1
                    right -= 1
                elif temp < 0:
                    left += 1
                else:
                    right -= 1
        return res
```


## 连续子数组的最大和
> [Leetcode 53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)
>
> [Leetcode 剑指 Offer 42. 连续子数组的最大和](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

> 给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**解析**：题目要求子数组不为空，所以连续子数组的和可以为负。

> 参考 [Python实现 《算法导论 第三版》中的算法 第4章 分治策略](https://blog.csdn.net/shengchaohua163/article/details/82810189)

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        max_sum = -float('inf')
        temp = 0
        for num in nums:
            temp += num
            if temp > max_sum:
                max_sum = temp
            if temp < 0:
                temp = 0

        return max_sum
```

如果子数组可以为空，一般规定空数组之和为0，所以连续子数组的和最小为0。
```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        max_sum = 0
        temp = 0
        for num in nums:
            temp += num
            if temp > max_sum:
                max_sum = temp
            elif temp < 0:
                temp = 0
        return max_sum
```


## 移动零
> [Leetcode 283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

> 给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。
>
> 说明: 1. 必须在原数组上操作，不能拷贝额外的数组。2. 尽量减少操作次数。

**解析**：保存一个下标，用来标识0的位置。

```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        last_zero_idx = -1
        for i in range(len(nums)):
            if nums[i] != 0:
                last_zero_idx += 1
                nums[i], nums[last_zero_idx] = nums[last_zero_idx], nums[i]
```

## 数组中的第K个最大元素
> [Leetcode 215. 数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

> 在未排序的数组中找到第 k 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

**解析**：使用快排的分割方法，分割数组后计算右边的元素个数。

```python
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        def partition(A, left, right):
            pivot = A[left]
            while left < right:
                while left < right and A[right] >= pivot:
                    right -= 1
                A[left] = A[right]
                while left < right and A[left] < pivot:
                    left += 1
                A[right] = A[left]
            A[left] = pivot
            return left
        
        def helper(A, left, right, k):
            if left <= right:
                p = partition(A, left, right)
                count = right - p + 1
                if count > k:
                    return helper(A, p + 1, right, k)
                elif count < k:
                    return helper(A, left, p - 1, k - count)
                return A[p]
            
        return helper(nums, 0, len(nums)-1, k)
```

## 分割数组
> [Leetcode 915. 分割数组](https://leetcode-cn.com/problems/partition-array-into-disjoint-intervals/)

> 给定一个数组 A，将其划分为两个不相交（没有公共元素）的连续子数组 left 和 right，使得：
> - left 中的每个元素都小于或等于 right 中的每个元素。
> - left 和 right 都是非空的。
> - left 要尽可能小。

> 在完成这样的分组后返回 left 的长度。可以保证存在这样的划分方法。

**解析**：记录左边数组的最大值和遍历过的所有元素的最大值。

```python
class Solution:
    def partitionDisjoint(self, A: List[int]) -> int:
        if not A:
            return 0

        left_max = A[0]
        cur_max = A[0]
        index = 0

        for i in range(1, len(A)):
            cur_max = max(cur_max, A[i])
            if A[i] < left_max:
                left_max = cur_max
                index = i

        return index + 1
```

## 非递减数列
> [Leetcode 665. 非递减数列](https://leetcode-cn.com/problems/non-decreasing-array/)

> 给你一个长度为 n 的整数数组，请你判断在 **最多** 改变 1 个元素的情况下，该数组能否变成一个非递减数列。
>
> 我们是这样定义一个非递减数列的：对于数组中所有的 i (0 <= i <= n-2)，总满足 nums[i] <= nums[i + 1]。

**解析**：需要考虑两种情况。

> 参考 [Leetcode-码不停题](https://leetcode-cn.com/problems/non-decreasing-array/comments/59727)

```python
class Solution:
    def checkPossibility(self, nums: List[int]) -> bool:
        if not nums or len(nums) <= 2:
            return True

        count = 0
        for i in range(1, len(nums)):
            if nums[i - 1] <= nums[i]:
                continue
            
            count += 1
            if count >= 2:
                return False

            if i >= 2 and nums[i - 2] > nums[i]:
                nums[i] = nums[ i - 1]
            else:
                nums[i - 1] = nums[i]
        
        return True
```

## 找到所有数组中消失的数字
> [Leetcode 448. 找到所有数组中消失的数字](https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/)

> 给定一个范围在  1 ≤ a[i] ≤ n ( n = 数组大小 ) 的 整型数组，数组中的元素一些出现了两次，另一些只出现一次。
>
> 找到所有在 [1, n] 范围之间没有出现在数组中的数字。
>
> 您能在不使用额外空间且时间复杂度为O(n)的情况下完成这个任务吗? 你可以假定返回的数组不算在额外空间内。

**解析**：把数组下标和数组中的数字联系在一起：把下标为`i`的元素「标记」为负数，表示整数`i+1`在数组中存在。

```python
class Solution:
    def findDisappearedNumbers(self, nums: List[int]) -> List[int]:
        for x in nums: 
            if nums[abs(x) - 1] > 0:
                nums[abs(x) - 1] *= -1
        res = []
        for i in range(len(nums)):
            if nums[i] > 0:
                res.append(i + 1)
        return res
```


## 数组中重复的数据
> [Leetcode 442. 数组中重复的数据](https://leetcode-cn.com/problems/find-all-duplicates-in-an-array/)

> 给定一个整数数组 a，其中1 ≤ a[i] ≤ n （n为数组长度）, 其中有些元素出现两次而其他元素出现一次。
>
> 找到所有出现两次的元素。
>
> 你可以不用到任何额外空间并在O(n)时间复杂度内解决这个问题吗？

**解析**：把数组下标和数组中的数字联系在一起：把下标为`i`的元素「标记」为负数，表示整数`i+1`在数组中存在。

```python
class Solution:
    def findDuplicates(self, nums: List[int]) -> List[int]:
        res = []
        for x in nums:
            if nums[abs(x) - 1] > 0:
                nums[abs(x) - 1] *= -1
            else:
                res.append(abs(x))
        return res
```


## 数组中重复的数字
> [Leetcode 剑指 Offer 03. 数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

> 找出数组中重复的数字。
>
> 在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。
>
> 示例1：
>
> ```
> 输入：[2, 3, 1, 0, 2, 5, 3]
> 输出：2 或 3 
> ```

**解析**：考察沟通能力。

1、时间优先，使用哈希表，代码略。此时时间复杂度为O(n)，空间复杂度O(n)。

2、空间优先，使用原地排序，代码略。此时时间复杂度最好为O(nlgn)，空间复杂度为O(1)。

3、 空间优先，但不允许排序，有三种方法：
1. 把每个数字都加1，此时和**Leetcode 442**相同，代码略；
2. 单独判断0是否重复，其他数字不用变化，并把0修改为一个比较大的数字，代码如下所示：
```python
class Solution:
    def findRepeatNumber(self, nums: List[int]) -> int:
        n = len(nums)
        zero_flag = False
        for i in range(n):
            if nums[i] == 0:
                if zero_flag: 
                    return 0
                zero_flag = True
                nums[i] = n
        for i in range(n):
            idx = abs(nums[i])
            if idx >= n: 
                continue
            if nums[idx] < 0:
                return idx
            nums[idx] = -abs(nums[idx])
        return -1
```
3. 原地置换，相当于排序，代码如下所示：
```python
class Solution:
    def findRepeatNumber(self, nums: List[int]) -> int:
        n = len(nums)
        for i in range(n):
            while nums[i] != i:
                if nums[i] == nums[nums[i]]:
                    return nums[i]
                temp = nums[i]
                nums[i], nums[temp] = nums[temp], nums[i]
                # print(nums)
        return -1
```

此时时间复杂度为O(n)，空间复杂度O(1)。

4、空间优先，只允许使用O(1)的空间，不允许修改数组。查看**Leetcode 287**。


## 寻找重复数
> [Leetcode 287. 寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number/)

> 给定一个包含 n + 1 个整数的数组 nums，其数字都在 1 到 n 之间（包括 1 和 n），可知至少存在一个重复的整数。假设只有一个重复的整数，找出这个重复的数。
>
> 示例 1:
>
> ```
> 输入: [1,3,4,2,2]
> 输出: 2
> ```
>
> 说明：
>
> - 不能更改原数组（假设数组是只读的）。
> - 只能使用额外的 O(1) 的空间。
> - 时间复杂度小于 O(n2) 。
> - 数组中只有一个重复的数字，但它可能不止重复出现一次。

**解析**：==TODO==

> 参考 [Leetcode官方题解](https://leetcode-cn.com/problems/find-the-duplicate-number/solution/xun-zhao-zhong-fu-shu-by-leetcode-solution/)


## 缺失的第一个正数
> [Leetcode 41. 缺失的第一个正数](https://leetcode-cn.com/problems/first-missing-positive/)

> 给你一个未排序的整数数组，请你找出其中没有出现的最小的正整数。
>
> 提示：你的算法的时间复杂度应为O(n)，并且只能使用常数级别的额外空间。

**解析**：最简单的方法是使用哈希表，但是不满足空间复杂度。

仔细思考，结果必然在`1`到`N+1`之间，因为数组最多包含`N`个整数。所以，可以把数组下标和数组中的数字联系在一起：把下标为`i`的元素「标记」为负数，表示整数`i+1`在数组中存在。

由于我们只在意`[1,N]`中的数，因此我们可以先对数组进行遍历，把不在`[1,N]`范围内的数修改成任意一个大于`N`的数（例如`N+1`）。

> 参考 [Leetdcode官方题解](https://leetcode-cn.com/problems/first-missing-positive/solution/que-shi-de-di-yi-ge-zheng-shu-by-leetcode-solution/)

```python
class Solution:
    def firstMissingPositive(self, nums: List[int]) -> int:
        n = len(nums)
        for i in range(n):
            if nums[i] <= 0:
                nums[i] = n + 1
        
        for i in range(n):
            num = abs(nums[i])
            if num <= n:
                nums[num - 1] = -abs(nums[num - 1])
        
        for i in range(n):
            if nums[i] > 0:
                return i + 1
        
        return n + 1
```


## 和为K的子数组
> [Leetcode 560. 和为K的子数组](https://leetcode-cn.com/problems/subarray-sum-equals-k/)

> 给定一个整数数组和一个整数 k，你需要找到该数组中和为 k 的连续的子数组的个数。

**解析**：

1、暴力法，时间复杂度$O(n^2)$。代码略。

2、前缀和 + 哈希表优化，时间复杂度$O(n)$。
```python
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        res = 0
        cumu_sum = 0  # 前缀和，累计和
        seen = {0:1}
        for num in nums:
            cumu_sum += num
            if cumu_sum - k in seen:
                res += seen[cumu_sum - k]
            if cumu_sum not in seen:
                seen[cumu_sum] = 0
            seen[cumu_sum] += 1
        
        return res
```


## 连续的子数组和
> [Leetcode 523. 连续的子数组和](https://leetcode-cn.com/problems/continuous-subarray-sum/)

> 给定一个包含 非负数 的数组和一个目标 整数`k`，编写一个函数来判断该数组是否含有连续的子数组，其大小至少为 2，且总和为 k 的倍数，即总和为 n*k，其中 n 也是一个整数。

> 示例 1：
```
输入：[23,2,4,6,7], k = 6
输出：True
解释：[2,4] 是一个大小为 2 的子数组，并且和为 6。
```

**解析**

1、暴力，时间复杂度`O(n2)`。
```python
class Solution:
    def checkSubarraySum(self, nums: List[int], k: int) -> bool:
        if len(nums) < 2:
            return False
        n = len(nums)
        for i in range(n - 1):
            cumu_sum = nums[i]
            for j in range(i + 1, n):
                cumu_sum += nums[j]
                if cumu_sum == 0:
                    return True
                if k != 0 and cumu_sum % k == 0:
                    return True
        return False
```

2、哈希表

> 参考 [Leetcode官方题解](https://leetcode-cn.com/problems/continuous-subarray-sum/solution/lian-xu-de-zi-shu-zu-he-by-leetcode/)

==TODO==



## 和可被 K 整除的子数组
> [Leetcode 974. 和可被 K 整除的子数组](https://leetcode-cn.com/problems/subarray-sums-divisible-by-k/)

> 给定一个整数数组`A`，返回其中元素之和可被`K`整除的（连续、非空）子数组的数目。
>
> 示例：
>
> ```
> 输入：A = [4,5,0,-2,-3,1], K = 5
> 输出：7
> 解释：
> 有 7 个子数组满足其元素之和可被 K = 5 整除：
> [4, 5, 0, -2, -3, 1], [5], [5, 0], [5, 0, -2, -3], [0], [0, -2, -3], [-2, -3]
> ```
>

**解析**：

> 参考 [Leetcode官方题解](https://leetcode-cn.com/problems/subarray-sums-divisible-by-k/solution/he-ke-bei-k-zheng-chu-de-zi-shu-zu-by-leetcode-sol/)

1、暴力，超时

```python
class Solution:
    def subarraysDivByK(self, A: List[int], K: int) -> int:
        res = 0
        n = len(A)
        for i in range(n):
            cumu_sum = 0
            for j in range(i, n):
                cumu_sum += A[j]
                if cumu_sum % K == 0:
                    res += 1
        return res
```

2、哈希表 + 前缀和
```python
class Solution:
    def subarraysDivByK(self, A: List[int], K: int) -> int:
        saved = {}
        saved[0] = 1
        res = 0
        cumu_sum = 0
        for num in A:
            cumu_sum += num
            mod = cumu_sum % K
            same = saved.get(mod, 0)
            res += same
            saved[mod] = same + 1
        return res
```

## 多数元素/数组中出现次数超过一半的数字

> [Leetcode 169. 多数元素](https://leetcode-cn.com/problems/majority-element/)
>
> [Leetcode 剑指 Offer 39. 数组中出现次数超过一半的数字](https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/)

> 给定一个大小为 n 的数组，找到其中的多数元素。多数元素是指在数组中出现次数大于 ⌊ n/2 ⌋ 的元素。
>
> 你可以假设数组是非空的，并且给定的数组总是存在多数元素。

**解析**：记录元素个数。

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        res = None
        count = 0
        for n in nums:
            if count == 0:
                res = n
                count = 1
            elif res == n:
                count += 1
            elif res != n:
                count -= 1
        return res
```


## 螺旋矩阵
> [Leetcode 54. 螺旋矩阵](https://leetcode-cn.com/problems/spiral-matrix/)
>
> [Leetcode 剑指 Offer 29. 顺时针打印矩阵](https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)

> 给定一个包含 m x n 个元素的矩阵（m行,n列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。

**解析**：

```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        if not matrix:
            return []
            
        m, n = len(matrix), len(matrix[0])
        min_row, max_row = 0, m - 1
        min_col, max_col = 0, n - 1
        res = []
        while True:
            for j in range(min_col, max_col + 1):
                res.append(matrix[min_row][j])
            min_row += 1
            if min_row > max_row:
                break
            for i in range(min_row, max_row + 1):
                res.append(matrix[i][max_col])
            max_col -= 1
            if max_col < min_col:
                break
            for j in range(max_col, min_col - 1, -1):
                res.append(matrix[max_row][j])
            max_row -= 1
            if max_row < min_row:
                break
            for i in range(max_row, min_row - 1, -1):
                res.append(matrix[i][min_col])
            min_col += 1
            if min_col > max_col:
                break

        return res
```


## 螺旋矩阵 II
> [Leetcode 59. 螺旋矩阵 II](https://leetcode-cn.com/problems/spiral-matrix-ii/)

> 给定一个正整数 n，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的正方形矩阵。
>
> 示例：
>
> ```
> 输入: 3
> 输出:
> [
>  [ 1, 2, 3 ],
>  [ 8, 9, 4 ],
>  [ 7, 6, 5 ]
> ]
> ```

**解析**：

```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        res = [[0] * n for _ in range(n)]
        num = 1 
        min_row, max_row = 0, n - 1
        min_col, max_col = 0, n - 1
        while True:
            for j in range(min_col, max_col + 1):
                res[min_row][j] = num
                num += 1
            min_row += 1
            if min_row > max_row:
                break
            for i in range(min_row, max_row + 1):
                res[i][max_col] = num
                num += 1
            max_col -= 1
            if max_col < min_col:
                break
            for j in range(max_col, min_col - 1, -1):
                res[max_row][j] = num
                num += 1
            max_row -= 1
            if max_row < min_row:
                break
            for i in range(max_row, min_row - 1, -1):
                res[i][min_col] = num
                num += 1
            min_col += 1
            if min_col > max_col:
                break

        return res
```


## 数组中的最长山脉
> [Leetcode 845. 数组中的最长山脉](https://leetcode-cn.com/problems/longest-mountain-in-array/)

> 我们把数组 A 中符合下列属性的任意连续子数组 B 称为 “山脉”：
> - B.length >= 3
> - 存在 0 < i < B.length - 1 使得 B[0] < B[1] < ... B[i-1] < B[i] > B[i+1] > ... > B[B.length - 1]
（注意：B 可以是 A 的任意子数组，包括整个数组 A。）

> 给出一个整数数组 A，返回最长 “山脉” 的长度。
>
> 如果不含有 “山脉” 则返回 0。
>
> 示例 1：
>
> ```
> 输入：[2,1,4,7,3,2,5]
> 输出：5
> 解释：最长的 “山脉” 是 [1,4,7,3,2]，长度为 5。
> ```

**解析**：

> 参考 [Leetcode 官方题解](https://leetcode-cn.com/problems/longest-mountain-in-array/solution/shu-zu-zhong-de-zui-chang-shan-mai-by-leetcode/)

```
class Solution:
    def longestMountain(self, A: List[int]) -> int:
        res = 0
        left = 0
        N = len(A)
        while left < N:
            end = left
            if end + 1 < N and A[end] < A[end + 1]: 
                while end + 1 < N and A[end] < A[end + 1]:
                    end += 1
                if end + 1 < N and A[end] > A[end + 1]:
                    while end + 1 < N and A[end] > A[end + 1]:
                        end += 1
                    res = max(res, end - left + 1)
            left = max(left + 1, end)
        return res
```



# 链表

## 反转链表

### 反转链表

> [Leetcode 206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

> 反转一个单链表。

**解析**：

1、普通

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

2、插入法

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

3、递归

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


### 反转链表 II

> [Leetcode 92. 反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/comments/)

> 反转从位置 m 到 n 的链表。请使用一趟扫描完成反转。

> 说明: 1 ≤ m ≤ n ≤ 链表长度。

**解析**：找到第 m - 1 个结点，插入法。

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


### 两两交换链表中的节点

> [Leetcode 24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

> 给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

> 你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

**解析**：

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


### K 个一组翻转链表

> [Leetcode 25. K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

> 给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。

> k 是一个正整数，它的值小于或等于链表的长度。

> 如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。

**解析**：插入法。

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


## 链表回文

> [Leetcode 234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)

> 请判断一个链表是否为回文链表。

**解析**：快慢指针，并对前半部分进行翻转。

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


## 旋转链表

> [Leetcode 61. 旋转链表](https://leetcode-cn.com/problems/rotate-list/)

> 给定一个链表，旋转链表，将链表每个节点向右移动 k 个位置，其中 k 是非负数。

**解析**：遍历求链表总长度，将链表首尾相连。

> 参考 [Leetcode-Gallianoo](https://leetcode-cn.com/u/gallianoo/)

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
        
        cur.next = head  # 首尾相连
        for i in range(count - k):
            cur = cur.next

        nxt = cur.next
        cur.next = None
        return nxt
```


## 合并两个有序链表

> [Leetcode 21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

> 将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

**解析**：

1、循环

```python
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        fake = ListNode(None)
        p = fake

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
        return fake.next
```

2、递归

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


## 链表排序

### 对链表进行插入排序

> [Leetcode 147. 对链表进行插入排序](https://leetcode-cn.com/problems/insertion-sort-list/)

**解析**：使用 next 指针比较方便，不需要保存当前结点的前一个结点。

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
            # 插入法，把nxt插在pre后面
            nxt = cur.next
            cur.next = nxt.next
            nxt.next = pre.next
            pre.next = nxt

        return dummy.next
```

### 排序链表 - O(nlgn)

> [Leetcode 148. 排序链表](https://leetcode-cn.com/problems/sort-list/)

> 在 O(n log n) 时间复杂度和常数级空间复杂度下，对链表进行排序。

**解析**：

1、使用归并排序。

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

2、使用快速排序。快速排序的第三种方法，选择头结点作为轴元素，因为选尾结点需要遍历一遍链表。

> 参考 [Leetcode-a380922457](https://leetcode-cn.com/problems/sort-list/solution/gui-bing-pai-xu-he-kuai-su-pai-xu-by-a380922457/)

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

### 合并K个有序链表

> [Leetcode 23. 合并K个排序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

> 合并 k 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。

**解析**：

> 参考 [LeetCode-Solution](https://leetcode-cn.com/problems/merge-k-sorted-lists/solution/he-bing-kge-pai-xu-lian-biao-by-leetcode-solutio-2/)

1、一个一个合并。

```python
class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        def merge_two_sorted_list(l1, l2):
            # 与 排序链表 O(nlgn)-归并排序 相同
            pass

        ans = None
        for l in lists:
            ans = merge_two_sorted_list(ans, l)

        return ans
```

分析：

- 时间复杂度：假设每个链表的最长长度是 $n$。在第一次合并后，ans的长度为 $n$；第二次合并后，ans的长度为 $2\times n$。第 $i$ 次合并后，ans的长度为 $i \times n$。第 $i$ 次合并的时间代价是 $O(n + (i - 1) \times n) = O(i \times n)$，那么总的时间代价为 $O(\sum_{i = 1}^{k} (i \times n)) = O(\frac{(1 + k)\cdot k}{2} \times n)$，故渐进时间复杂度为 $O(k^2 n)$。
- 空间复杂度：没有用到与 $k$ 和 $n$ 规模相关的辅助空间，故渐进空间复杂度为 $O(1)$。

2、分治法，与归并排序的分治相同。

```python
class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        def merge_two_sorted_list(l1, l2):
            # 与 排序链表 O(nlgn)-归并排序 相同
            pass

        def helper(lists):
            if len(lists) == 0:
                return None
            elif len(lists) == 1:
                return lists[0]
            
            mid = len(lists) // 2
            left = helper(lists[:mid])
            right = helper(lists[mid:])
            return merge_two_sorted_list(left, right)

        return helper(lists)
```

分析：

- 时间复杂度：考虑递归「向上回升」的过程——第一轮合并 $\frac{k}{2} $ 组链表，每一组的时间代价是 $O(2n)$；第二轮合并 $\frac{k}{4} $组链表，每一组的时间代价是 $O(4n)$......所以总的时间代价是 $O(\sum_{i = 1}^{\infty} \frac{k}{2^i} \times 2^i n)$，故渐进时间复杂度为 $O(kn \times \log k)$。
- 空间复杂度：递归会使用到 $O(\log k)$ 空间代价的栈空间。


## 两数相加

> [Leetcode 2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

> 给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 **逆序** 的方式存储的，并且它们的每个节点只能存储 一位 数字。
>
> 如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。
>
> 您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

**解析**：

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


## 两数相加 II

> [Leetcode 445. 两数相加 II](https://leetcode-cn.com/problems/add-two-numbers-ii/)

> 给你两个 非空 链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。
>
> 你可以假设除了数字 0 之外，这两个数字都不会以零开头。
>
> 进阶：如果输入链表不能修改该如何处理？换句话说，你不能对列表中的节点进行翻转。

**解析**：栈！

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

## 重排链表

> [Leetcode 143. 重排链表](https://leetcode-cn.com/problems/reorder-list/)

> 给定一个单链表 L：L0→L1→…→Ln-1→Ln ，
> 将其重新排列后变为： L0→Ln→L1→Ln-1→L2→Ln-2→…
>
> 你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

**解析**：

```python
class Solution:
    def reorderList(self, head: ListNode) -> None:
        """
        Do not return anything, modify head in-place instead.
        """
        if not head or not head.next:
            return
        # 快慢指针，确定重点
        slow, fast = head, head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        right = slow.next
        slow.next = None
        # 右半部分链表反转
        pre, cur = None, right 
        while cur:
            temp = cur.next 
            cur.next = pre
            pre = cur
            cur = temp
        # 拼接两个链表
        left, right = head, pre
        while left and right:
            temp2 = left.next
            temp3 = right.next
            left.next = right
            right.next = temp2
            left = temp2
            right = temp3
```



# 栈
## 基本计算器
> [Leetcode 224. 基本计算器](https://leetcode-cn.com/problems/basic-calculator/)

> 实现一个基本的计算器来计算一个简单的字符串表达式的值。
>
> 字符串表达式可以包含左括号`(`，右括号`)`，加号`+`，减号`-`，非负整数和空格` `。

**解析**：

==TODO==

## 验证栈序列
> [Leetcode 946. 验证栈序列](https://leetcode-cn.com/problems/validate-stack-sequences/)

> 给定 pushed 和 popped 两个序列，每个序列中的 值都不重复，只有当它们可能是在最初空栈上进行的推入 push 和弹出 pop 操作序列的结果时，返回 true；否则，返回 false。

**解析**：使用栈模拟。

```python
class Solution:
    def validateStackSequences(self, pushed: List[int], popped: List[int]) -> bool:
        stack = []
        index = 0
        for num in pushed:
            stack.append(num)
            while stack and stack[-1] == popped[index]:
                stack.pop()
                index += 1
                
        return not stack
```

## 最小栈
> [Leetcode 155. 最小栈](https://leetcode-cn.com/problems/min-stack/)

> 设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。
> - push(x) —— 将元素 x 推入栈中。
> - pop() —— 删除栈顶的元素。
> - top() —— 获取栈顶元素。
> - getMin() —— 检索栈中的最小元素。

**解析**：增加一个辅助栈，用来保存当前栈内的最小元素。

```python
class MinStack:

    def __init__(self):
        self.stack = []
        self.aux_stack = []

    def push(self, x: int) -> None:
        self.stack.append(x)
        if len(self.aux_stack) == 0:
            self.aux_stack.append(x)
        else:
            cur_min = self.aux_stack[-1]
            self.aux_stack.append(min(cur_min, x))

    def pop(self) -> None:
        if self.stack:
            self.stack.pop()
            self.aux_stack.pop()

    def top(self) -> int:
        if self.stack:
            return self.stack[-1]
        return 0

    def min(self) -> int:
        if self.aux_stack:
            return self.aux_stack[-1]
```


## 检查替换后的词是否有效
> [Leetcode 1003. 检查替换后的词是否有效](https://leetcode-cn.com/problems/check-if-word-is-valid-after-substitutions/)

**解析**：使用栈模拟。

```python
class Solution:
    def isValid(self, S: str) -> bool:
        stack = []
        for ch in S:
            if ch == 'a':
                stack.append(ch)
            elif ch == 'b':
                if stack and stack[-1] == 'a':
                    stack.append(ch)
                else:
                    return False
            elif ch == 'c':
                if stack and stack[-1] == 'b':
                    stack.pop()
                    stack.pop()
                else:
                    return False

        return len(stack) == 0
```


## 有效的括号（括号匹配）
> [Leetcode 20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

> 给定一个只包括'('，')'，'{'，'}'，'['，']'的字符串，判断字符串是否有效。
>
> 有效字符串需满足：
>
> 1. 左括号必须用相同类型的右括号闭合。
> 2. 左括号必须以正确的顺序闭合。

> 注意空字符串可被认为是有效字符串。

**解析**：

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        for c in s:
            if c == '(' or c == '[' or c == '{':
                stack.append(c)
            else:
                if not stack:
                    return False             
                elif stack[-1] == '(' and c == ')' or stack[-1] == '[' and c == ']' or stack[-1] == '{' and c == '}':
                    stack.pop()
                else:
                    return False
        return len(stack) == 0
```


## 括号的分数
> [Leetcode 856. 括号的分数](https://leetcode-cn.com/problems/score-of-parentheses/)

> 给定一个平衡括号字符串 S，按下述规则计算该字符串的分数：
>
> - () 得 1 分。
> - AB 得 A + B 分，其中 A 和 B 是平衡括号字符串。
> - (A) 得 2 * A 分，其中 A 是平衡括号字符串。

**解析**：使用栈模拟，遇到左括号就添加一个0，遇到右括号就弹出一个元素，并修改最后一个元素。

> 参考 [Leetcode官方题解](https://leetcode-cn.com/problems/score-of-parentheses/solution/gua-hao-de-fen-shu-by-leetcode/)

```python
class Solution:
    def scoreOfParentheses(self, S):
        stack = [0]  # The score of the current frame
        for x in S:
            if x == '(':
                stack.append(0)
            else:
                v = stack.pop()
                stack[-1] += max(2 * v, 1)

        return stack[0]
```


## 使括号有效的最少添加
> [Leetcode 921. 使括号有效的最少添加](https://leetcode-cn.com/problems/minimum-add-to-make-parentheses-valid/)

> 给定一个由`(`和`)`括号组成的字符串S，我们需要添加最少的括号（ `(`或是`)`，可以在任何位置），以使得到的括号字符串有效。

**解析**：使用栈模拟。

```python
class Solution:
    def minAddToMakeValid(self, S: str) -> int:
        stack = []
        num = 0
        for c in S:
            if c == '(':
                stack.append(c)
            elif c == ')' :
                if stack and stack[-1] == '(':
                    stack.pop()
                else:
                    num += 1
        
        return num + len(stack)
```


## 最长有效括号
> [Leetcode 32. 最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)

> 给定一个只包含 '(' 和 ')' 的字符串，找出最长的包含有效括号的子串的长度。

**解析**：
1. 始终保持栈底元素为当前已经遍历过的元素中「最后一个没有被匹配的右括号的下标」；
2. 栈初始化包含一个`-1`，栈里其他元素用于维护左括号的下标：
    - 对于遇到的每个'('，将它的下标放入栈中；
    - 对于遇到的每个')'，先弹出栈顶元素，表示匹配了栈顶元素，弹出栈之后：
        - 如果栈为空，说明当前的右括号为没有被匹配的右括号，我们将其下标放入栈中来更新我们之前提到的「最后一个没有被匹配的右括号的下标」
        - 如果栈不为空，当前右括号的下标减去栈顶元素即为「以该右括号为结尾的最长有效括号的长度」

> 参考 [Leetcode官方题解](https://leetcode-cn.com/problems/longest-valid-parentheses/solution/zui-chang-you-xiao-gua-hao-by-leetcode-solution/)

```python
class Solution:
    def longestValidParentheses(self, s: str) -> int:
        res = 0
        stack = [-1]
        for i, ch in enumerate(s):
            if ch == '(':
                stack.append(i)
            else:
                stack.pop()
                if not stack:
                    stack.append(i)
                else:
                    res = max(res, i - stack[-1])
        return res
```


## 每日温度
> [Leetcode 739. 每日温度](https://leetcode-cn.com/problems/daily-temperatures/)

> 请根据每日`气温`列表，重新生成一个列表。对应位置的输出为：要想观测到更高的气温，至少需要等待的天数。如果气温在这之后都不会升高，请在该位置用 0 来代替。
>
> 例如，给定一个列表 temperatures = [73, 74, 75, 71, 69, 72, 76, 73]，你的输出应该是 [1, 1, 4, 2, 1, 1, 0, 0]。

**解析**：使用栈模拟。

```python
class Solution:
    def dailyTemperatures(self, T: List[int]) -> List[int]:
        res = [0] * len(T)
        stack = []
        for i, temper in enumerate(T):
            while stack and T[stack[-1]] < temper:
                idx = stack.pop()
                res[idx] = i - idx
            stack.append(i)

        return res
```

## 下一个更大元素 I
> [Leetcode 496. 下一个更大元素 I](https://leetcode-cn.com/problems/next-greater-element-i/)

> 给定两个**没有重复元素**的数组`nums1`和`nums2`，其中`nums1`是`nums2`的子集。找到`nums1`中每个元素在`nums2`中的下一个比其大的值。

**解析**：先只考虑第二个数组即可。

```python
class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        stack = []
        seen = {}
        for num in nums2:
            while stack and stack[-1] < num:
                temp = stack.pop()
                seen[temp] = num
            stack.append(num)
        for num in stack:
            seen[num] = - 1

        return [seen[num] for num in nums1]
```


## 下一个更大元素 II
> [Leetcode 503. 下一个更大元素 II](https://leetcode-cn.com/problems/next-greater-element-ii/)

> 给定一个循环数组（最后一个元素的下一个元素是数组的第一个元素），输出每个元素的下一个更大元素。数字 x 的下一个更大的元素是按数组遍历顺序，这个数字之后的第一个比它更大的数，这意味着你应该循环地搜索它的下一个更大的数。如果不存在，则输出 -1。

**解析**：遍历两次。

```python
class Solution:
    def nextGreaterElements(self, nums: List[int]) -> List[int]:
        stack = []
        seen = {}
        double_range = list(range(len(nums))) * 2
        for i in double_range:
            while stack and nums[stack[-1]] < nums[i]:
                idx = stack.pop()
                if idx not in seen:
                    seen[idx] = nums[i]
            stack.append(i)

        for i in stack:
            if i not in seen:
                seen[i] = -1

        return [seen[i] for i in range(len(nums))]
```



# 队列

## 用两个栈实现队列

>  [剑指 Offer 09. 用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

> 用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 `appendTail` 和 `deleteHead` ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，`deleteHead` 操作返回 -1 )

**解析**：两个栈，一个用于入队，一个用于出队。

```python
class CQueue:
    def __init__(self):
        self.stack1 = []
        self.stack2 = []

    def appendTail(self, value: int) -> None:
        self.stack1.append(value)

    def deleteHead(self) -> int:
        if len(self.stack2) == 0:
            while self.stack1:
                self.stack2.append(self.stack1.pop())
        if len(self.stack2) == 0:
            return -1
        return self.stack2.pop()
```

## 滑动窗口最大值

> [Leetcode 239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

> 给定一个数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。
>
> 返回滑动窗口中的最大值。

**解析**：

1. 双端队列作为滑动窗口；
2. 队列中存放的是数组下标，最左边的位置存放的是当前窗口中最大元素的下标；
3. 如果当前队列大小等于指定大小（`i - queue[0] >= k`），则无法再添加元素，必须从左边弹出（`queue.popleft()`）；
4. 如果当前队列末尾存放的下标对应的元素小于要入队的下标对应的元素（`nums[queue[-1]] < nums[i]`），则从队尾出队（`queue.pop()`）；
5. 当前下标入队（`queue.append(i)`）。如果下标已经达到指定窗口大小，保存当前窗口的最大值。

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
            if i >= k - 1:
                res.append(nums[queue[0]])
        
        return res
```


## 滑动窗口中位数
> [Leetcode 480. 滑动窗口中位数](https://leetcode-cn.com/problems/sliding-window-median/)

> 中位数是有序序列最中间的那个数。如果序列的大小是偶数，则没有最中间的数；此时中位数是最中间的两个数的平均数。
>
> 例如：
>
> - [2,3,4]，中位数是 3
> - [2,3]，中位数是 (2 + 3) / 2 = 2.5

> 给你一个数组 nums，有一个大小为 k 的窗口从最左端滑动到最右端。窗口中有 k 个数，每次窗口向右移动 1 位。你的任务是找出每次窗口移动后得到的新窗口中元素的中位数，并输出由它们组成的数组。


**解析**：二分法、堆。

1、二分法
```python
class Solution:
    def medianSlidingWindow(self, nums: List[int], k: int) -> List[float]:
        res = []
        window = []
        low, high = 0, 0
        while high < len(nums):
            bisect.insort_left(window, nums[high])
            while len(window) > k:
                window.pop(bisect.bisect_left(window, nums[low]))  # 出窗
                low += 1
            if len(window) == k:
                res.append((window[k // 2] + window[(k - 1) // 2]) / 2)
            high += 1
        return res
```

2、堆

==TODO==



## 队列的最大值
> [Leetcode 剑指 Offer 59 - II. 队列的最大值](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/)

> 请定义一个队列并实现函数`max_value`得到队列里的最大值，要求函数`max_value`、`push_back`和`pop_front`的均摊时间复杂度都是O(1)。
>
> 若队列为空，`pop_front`和`max_value`需要返回 -1。

**解析**：类似于最小栈。

```python
class MaxQueue:

    def __init__(self):
        from collections import deque
        self.queue = deque()
        self.max_eles = deque()

    def max_value(self) -> int:
        return self.max_eles[0] if self.max_eles else -1

    def push_back(self, value: int) -> None:
        self.queue.append(value)
        while self.max_eles and value > self.max_eles[-1]:
            self.max_eles.pop()
        self.max_eles.append(value)

    def pop_front(self) -> int:
        if not self.queue:
            return -1
        val = self.queue.popleft()
        if val == self.max_eles[0]:
            self.max_eles.popleft()
        return val
```





# 字符串

## 实现 strStr() - 字符串搜索
> [Leetcode 28. 实现 strStr()](https://leetcode-cn.com/problems/implement-strstr/)

> 实现 strStr() 函数。
>
> 给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回 -1。

**解析**：

1、暴力搜索
```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        if not needle:
            return 0
        if not haystack:
            return -1
        
        for i in range(len(haystack) - len(needle) + 1):
            for j in range(len(needle)):
                if haystack[i + j] != needle[j]:
                    break
            else:
                return i
        
        return -1
```

2、KMP
> 参考 [小白之KMP算法详解及python实现](https://blog.csdn.net/weixin_39561100/article/details/80822208)

对于一个长度为`N`的字符串`S`：
- 前缀为第`0`个字符`S[0]`到到第`j`个字符`S[j]`组成的子串，其中`0<=j<=N-1`
- 后缀为第`j`个字符`S[j]`到到第`N-1`个字符`S[N-1]`组成的子串，其中`0<=j<=N-1`
- 真前（后）缀表示不包括字符串本身的前（后）缀

对于字符串`abcab`：
- 前缀包括`a,ab,abc,abca,abcab`，后缀包括`abcab,bcab,cab,ab,b`
- 真前缀包括`a,ab,abc,abca`，真后缀包括`bcab,cab,ab,b`

对于一个长度为`N`的字符串`S`，它的next指针数组的长度与字符串长度相等，其中第`i`个元素表示该字符串第`0`个字符`S[0]`到第`i`个字符`S[i]`组成的子串的最大相同真前后缀的长度。

对于字符串`abcab`，next指针数组为`[0,0,0,1,2]`。

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        def gen_next(string):
            N = len(string)
            next_pointer = [0] * N
            left = 0
            cur = 1
            while cur < N:
                if string[cur] == string[left]:
                    next_pointer[cur] = left + 1
                    left += 1
                    cur += 1
                elif left == 0:
                    next_pointer[cur] = 0
                    cur += 1
                else: # left != 0
                    left = next_pointer[left - 1]
            return next_pointer

        nextp = gen_next(needle)
        m = len(haystack)
        n = len(needle)
        i, j = 0, 0
        while i < m and j < n:
            if haystack[i] == needle[j]:
                i += 1
                j += 1
            elif j == 0:
                i += 1
            else:
                j = nextp[j - 1]
        if j == n:
            return i - j
        return -1
```


## 字符串转换整数 (atoi)
> [Leetcode 8. 字符串转换整数 (atoi)](https://leetcode-cn.com/problems/string-to-integer-atoi/)

> 请你来实现一个 atoi 函数，使其能将字符串转换成整数。

**解析**：不要使用`strip`函数。

```python
class Solution:
    def myAtoi(self, string: str) -> int:
        if not string or string[0].isalpha():
            return 0
        
        begin = 0
        res = 0
        sign = 1
        while begin < len(string) and string[begin] == " ":
            begin += 1
        
        if begin >= len(string):
            return 0
        
        if string[begin] == "+":
            begin += 1
        elif string[begin] == "-":
            sign = -1
            begin += 1
        
        while begin < len(string) and string[begin].isdigit():
            res = res * 10 + int(string[begin])
            begin += 1
        
        res = res * sign
        if res > 2 ** 31 - 1:
            return 2 ** 31 - 1
        elif res < -2 ** 31:
            return -2 ** 31
        return res
```


## 无重复字符的最长子串
> [Leetcode 3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

> 给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

**解析**：滑动窗口。

> 参考 [Leetcode-powcai](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/solution/hua-dong-chuang-kou-by-powcai/)

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        if not s: 
            return 0
        
        res = 0
        left = 0
        seen = set()
        for right in range(len(s)):
            while s[right] in seen:
                seen.remove(s[left])
                left += 1
            seen.add(s[right])
            res = max(res, right - left + 1)
        
        return res
```


## 去除重复字母/不同字符的最小子序列
> [Leetcode 316. 去除重复字母](https://leetcode-cn.com/problems/remove-duplicate-letters/)
>
> [Leetcode 1081. 不同字符的最小子序列](https://leetcode-cn.com/problems/smallest-subsequence-of-distinct-characters/)

> 给你一个仅包含小写字母的字符串，请你去除字符串中重复的字母，使得每个字母只出现一次。需保证返回结果的字典序最小（要求不能打乱其他字符的相对位置）。

> 示例 1:
>
> ```
> 输入: "bcabc"
> 输出: "abc"
> ```
**解析**：使用栈和哈希表。

> 参考 [Leetcode官方题解](https://leetcode-cn.com/problems/remove-duplicate-letters/solution/qu-chu-zhong-fu-zi-mu-by-leetcode/)

```python
class Solution:
    def removeDuplicateLetters(self, s) -> int:
        stack = []
        seen = set()
        last_occurrence = {c: i for i, c in enumerate(s)}
        for i, c in enumerate(s):
            if c not in seen:
                while stack and c < stack[-1] and i < last_occurrence[stack[-1]]:
                    seen.discard(stack.pop())
                seen.add(c)
                stack.append(c)
        return ''.join(stack)
```


## 罗马数字转整数
> [Leetcode 13. 罗马数字转整数](https://leetcode-cn.com/problems/roman-to-integer/)

> 罗马数字包含以下七种字符:`I`，`V`，`X`，`L`，`C`，`D`和`M`。
>
> 给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。

**解析**：

```python
class Solution:
    def romanToInt(self, s: str) -> int:
        roman_map = {'I': 1, 'V': 5, 'X': 10, 'L': 50, 'C': 100, 'D': 500, 'M': 1000}
        res = 0
        for i in range(len(s)):
            if i < len(s) - 1 and roman_map[s[i]] < roman_map[s[i + 1]]:
                res -= roman_map[s[i]]
            else:
                res += roman_map[s[i]]
                
        return res
```


## 整数转罗马数字
> [Leetcode 12. 整数转罗马数字](https://leetcode-cn.com/problems/integer-to-roman/)

> 给定一个整数，将其转为罗马数字。输入确保在 1 到 3999 的范围内。

**解析**：

```python
class Solution:
    def intToRoman(self, num: int) -> str:
        roman_numbers = {1:'I', 4:'IV', 5:'V', 9:'IX',
                         10:'X', 40: 'XL', 50:'L', 90:'XC', 
                         100:'C', 400:'CD', 500:'D', 900:'CM', 
                         1000:'M'}
        res = []
        for d in sorted(roman_numbers.keys(), reverse=True):
            (r, num) = divmod(num, d)
            if r == 0:
                continue
            res.append(roman_numbers[d]*r)
        return ''.join(res)
```


## 下一个更大元素 III
> [Leetcode 556. 下一个更大元素 III](https://leetcode-cn.com/problems/next-greater-element-iii/)

> 给定一个32位正整数 n，你需要找到最小的32位整数，其与 n 中存在的位数完全相同，并且其值大于n。如果不存在这样的32位整数，则返回-1。

**解析**：把数字转成字符数组，按字母表的顺序找到可以交换的位置。

```python
class Solution:
    def nextGreaterElement(self, n: int) -> int:
        chars = list(str(n))
        length = len(chars)
        right = length - 2
        while right >= 0 and chars[right] >= chars[right + 1]:
            right -= 1
        if right < 0:
            return -1
        # 在left右边，找到最后一个大于chars[left]的数字
        left = right
        right = right + 1
        while right < length and chars[right] > chars[left]:
            right += 1
        chars[left], chars[right - 1] = chars[right - 1], chars[left]
        # left右边的数组进行翻转
        begin = left + 1
        end = length - 1
        while begin < end:
            chars[begin], chars[end] = chars[end], chars[begin]
            begin += 1
            end -= 1
        # 计算
        res = 0
        for ch in chars:
            res = res * 10 + int(ch)
        
        return res if res < 2 ** 31 - 1 else -1
```

## 上一个更小元素

> 给定一个32位正整数 n，你需要找到最大的32位整数，其与 n 中存在的位数完全相同，并且其值小于n。如果不存在这样的32位整数，则返回-1。

**注意**：Leetcode没有这一题。

**解析**：把数字转成字符数组，按字母表的顺序找到可以交换的位置。

```python
class Solution:
    def lastSmallerElement(self, n: int) -> int:
        chars = list(str(n))
        length = len(chars)
        right = length - 2
        while right >= 0 and chars[right] <= chars[right + 1]:
            right -= 1
        if right < 0:
            return -1
        # 在left右边，找到最后一个小于chars[left]的数字
        left = right
        right = right + 1
        while right < length and chars[right] < chars[left]:
            right += 1
        chars[left], chars[right - 1] = chars[right - 1], chars[left]
        # 右边的数组进行翻转
        begin = left + 1
        end = length - 1
        while begin < end:
            chars[begin], chars[end] = chars[end], chars[begin]
            begin += 1
            end -= 1
        if chars[0] == "0":
            return -1
        res = 0
        for ch in chars:
            res = res * 10 + int(ch)
        return res
```

## 基本计算器 II

> [Leetcode 227. 基本计算器 II](https://leetcode-cn.com/problems/basic-calculator-ii/)

> 实现一个基本的计算器来计算一个简单的字符串表达式的值。
>
> 字符串表达式仅包含非负整数，+， - ，*，/ 四种运算符和空格  。 整数除法仅保留整数部分。

**解析**：

```python
class Solution:
    def calculate(self, s: str) -> int:
        res = 0
        sign = '+'
        digit = 0
        nums = []
        s = s.strip()
        for i in range(len(s)):
            if s[i] == ' ': 
                continue
            if s[i].isdigit(): 
                digit = digit * 10 + int(s[i])
            if not s[i].isdigit() or i == len(s) - 1:
                if sign == '+':
                    nums.append(digit)
                elif sign == '-':
                    nums.append(-digit)
                elif sign == '*':
                    tmp = nums[-1] * digit
                    nums[-1] = tmp
                elif sign == '/':
                    # print(nums, digit)
                    tmp = abs(nums[-1]) // digit
                    if nums[-1] < 0:
                        tmp = -tmp
                    nums[-1] = tmp
                digit = 0
                sign = s[i]
                
        return sum(nums)
```


## 压缩字符串
> [Leetcode 443. 压缩字符串](https://leetcode-cn.com/problems/string-compression/)

> 给定一组字符，使用原地算法将其压缩。
>
> 压缩后的长度必须始终小于或等于原数组长度。
>
> 数组的每个元素应该是长度为 1 的字符（不是 int 整数类型）。
>
> 在完成原地修改输入数组后，返回数组的新长度。
>
> 进阶：你能否仅使用O(1) 空间解决问题？
>

> 示例：
>
> ```
>输入：
> ["a","a","b","b","c","c","c"]
>
> 输出：
>返回 6 ，输入数组的前 6 个字符应该是：["a","2","b","2","c","3"]
> 
>说明：
> "aa" 被 "a2" 替代。"bb" 被 "b2" 替代。"ccc" 被 "c3" 替代。
>```

**解析**：

> 参考 [Leetcode官方题解](https://leetcode-cn.com/problems/string-compression/solution/ya-suo-zi-fu-chuan-by-leetcode/)

```python
class Solution(object):
    def compress(self, chars):
        anchor = write = 0
        for read, c in enumerate(chars):
            if read + 1 == len(chars) or chars[read + 1] != c:
                chars[write] = chars[anchor]
                write += 1
                if read > anchor:
                    for digit in str(read - anchor + 1):
                        chars[write] = digit
                        write += 1
                anchor = read + 1
        return write
```


## Z 字形变换
> [Leetcode 6. Z 字形变换](https://leetcode-cn.com/problems/zigzag-conversion/)

> 将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。

**解析**：设置一个flag变量。

```python
class Solution:
    def convert(self, s: str, numRows: int) -> str:
        if numRows == 1:
            return s
        
        res = [[] for _ in range(numRows)]
        row = 0
        flag = 1
        for c in s:
            res[row].append(c)
            if row == 0:
                flag = 1
            elif row == numRows - 1:
                flag = -1
            row += flag
        
        return ''.join(''.join(r) for r in res)
```


## 计数二进制子串
> [Leetcode 696. 计数二进制子串](https://leetcode-cn.com/problems/count-binary-substrings/)

> 给定一个字符串`s`，计算具有相同数量0和1的非空(连续)子字符串的数量，并且这些子字符串中的所有0和所有1都是组合在一起的。
>
> 重复出现的子串要计算它们出现的次数。

**解析**：

1、中心扩展法

```python
class Solution:
    def countBinarySubstrings(self, s: str) -> int:
        count = 0
        for i in range(len(s) - 1):
            if s[i] != s[i + 1]:
                count += 1
                for j in range(1, min(i + 1, len(s) - i - 1)):
                    if not (s[i - j] == s[i] and s[i + 1] == s[i + 1 + j]):
                        break
                    count += 1
                            
        return count
```

2、线性扫描
> 参考 [Leetcode官方题解](https://leetcode-cn.com/problems/count-binary-substrings/solution/ji-shu-er-jin-zhi-zi-chuan-by-leetcode/)

```python
class Solution(object):
    def countBinarySubstrings(self, s):
        ans, prev, cur = 0, 0, 1
        for i in range(1, len(s)):
            if s[i-1] != s[i]:
                ans += min(prev, cur)
                prev, cur = cur, 1
            else:
                cur += 1

        return ans + min(prev, cur)
```


## 字符串的最大公因子
> [Leetcode 1071. 字符串的最大公因子](https://leetcode-cn.com/problems/greatest-common-divisor-of-strings/)

> 对于字符串`S`和`T`，只有在`S = T + ... + T`（`T`与自身连接 1 次或多次）时，我们才认定“`T` 能除尽 `S`”。
>
> 返回最长字符串`X`，要求满足`X`能除尽`str1`且`X`能除尽`str2`。

**解析**：

```python
class Solution:
    def gcdOfStrings(self, str1: str, str2: str) -> str:
        def gcd(a, b):
            if b == 0:
                return a
            return gcd(b, a % b)
        
        if str1 + str2 != str2 + str1:
            return ''
        return str1[:gcd(len(str1), len(str2))]
```


## 字符串相加
> [Leetcode 415. 字符串相加](https://leetcode-cn.com/problems/add-strings/)

> 给定两个字符串形式的非负整数 num1 和num2 ，计算它们的和。

**解析**：

```python
class Solution:
    def addStrings(self, num1: str, num2: str) -> str:
        len1 = len(num1)
        len2 = len(num2)
        i = len1 - 1
        j = len2 - 1
        res = []
        carry = 0
        while i >= 0 or j >= 0 or carry != 0:
            d1 = int(num1[i]) if i >= 0 else 0
            d2 = int(num2[j]) if j >= 0 else 0
            temp = d1 + d2 + carry
            carry, temp = divmod(temp, 10)
            res.append(str(temp))
            i -= 1
            j -= 1
        return ''.join(res[::-1])
```


## 字符串相乘
> [Leetcode 43. 字符串相乘](https://leetcode-cn.com/problems/multiply-strings/)

> 给定两个以字符串形式表示的非负整数 num1 和 num2，返回 num1 和 num2 的乘积，它们的乘积也表示为字符串形式。

**解析**：

> 参考 [Leetcode 官方题解](https://leetcode-cn.com/problems/multiply-strings/solution/zi-fu-chuan-xiang-cheng-by-leetcode-solution/)

```python
class Solution:
    def multiply(self, num1: str, num2: str) -> str:
        if num1 == "0" or num2 == "0":
            return "0"
        
        m, n = len(num1), len(num2)
        ansArr = [0] * (m + n)
        for i in range(m - 1, -1, -1):
            x = int(num1[i])
            for j in range(n - 1, -1, -1):
                ansArr[i + j + 1] += x * int(num2[j])
        
        for i in range(m + n - 1, 0, -1):
            ansArr[i - 1] += ansArr[i] // 10
            ansArr[i] %= 10
        
        index = 1 if ansArr[0] == 0 else 0
        return "".join(str(x) for x in ansArr[index:])
```

## 替换后的最长重复字符

> [Leetcode 424. 替换后的最长重复字符](https://leetcode-cn.com/problems/longest-repeating-character-replacement/)

> 给你一个仅由大写英文字母组成的字符串，你可以将任意位置上的字符替换成另外的字符，总共可最多替换 *k* 次。在执行上述操作后，找到包含重复字母的最长子串的长度。

> **示例 1：**
>
> ```
> 输入：s = "ABAB", k = 2
> 输出：4
> 解释：用两个'A'替换为两个'B',反之亦然。
> ```

**解析**：

> 参考官方题解

==TODO==

## 汉字读法的整数转为阿拉伯数字

> 示例：

```
五千四百零三万一千二百
54031200
```

**注意**：非leetcode。

**解析**：不保证完全正确。

```python
def chinese2int(string):
    """string表示的数字小于一亿"""
    digits_map = {"零": 0, "一": 1, "二": 2, "三": 3, "四": 4, "五": 5, 
                  "六": 6, "七": 7, "八": 8, "九": 9, "十": 10}
    unit_map = {"千": 1000, "百": 100, "十": 10, "零": 0}
    if len(string) == 1:
        return digits_map[string[0]]
    num = 0
    i = 0
    N = len(string)
    while i < N:
        if i < N and string[i] == "万":
            num *= 10000
            i += 1
        if i == N - 1:
            num += digits_map[string[i]]
            i += 1
            break
        end = i
        while end < N and string[end] not in unit_map:
            end += 1
        if i == end:
            if string[end] == "零":
                end += 1
                temp = digits_map[string[end]]
                end += 1
                if end < N and string[end] == "十":
                    temp *= 10
                    end += 1
                num += temp
            elif string[end] == "十":
                num += 10
                end += 1
                num += digits_map[string[end]]
                end += 1
        elif end < N:
            num += digits_map[string[i]] * unit_map[string[end]]
            end += 1
        i = end
    return num


if __name__ == "__main__":
    test_cases = [["零", 0],
                  ["一", 1],
                  ["十", 10],
                  ["十一", 11],
                  ["二十", 20],
                  ["一百", 100],
                  ["一百零一", 101],
                  ["一百一十一", 111],
                  ["一千", 1000],
                  ["一千零一", 1001],
                  ["五千零二十万一千二百零五", 50201205]]
    for string, ans in test_cases:
        res = chinese2int(string)
        print(string, ans, res)
```



# 树
## 说明
如果没有特殊说明，二叉树和N叉树的表示如下所示：

1、二叉树
```python
class TreeNode:
    def __init__(self, x, left=None, right=None):
        self.val = x
        self.left = left
        self.right = right
```

2、N叉树
```
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children  # 列表
```

## 二叉树遍历
### 二叉树的前序遍历
> [Leetcode 144. 二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

> 给定一个二叉树，返回它的 前序 遍历。

1、递归版本
```python
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        def _preorder(node):
            if node:
                res.append(node.val)
                _preorder(node.left)
                _preorder(node.right)
        
        res = []
        _preorder(root)
        return res
```

2、非递归版本，推荐方法
```python
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        if not root:
            return []
        
        res = []
        stack = []
        node = root
        
        while stack or node:
            if node:
                stack.append(node)
                res.append(node.val)
                node = node.left
            else:
                node = stack.pop()
                node = node.right
        
        return res
```

3、非递归版本

```python
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        if not root:
            return []
        
        res = []
        stack = [root]
        
        while stack:
            node = stack.pop()
            res.append(node.val)
            if node.right:
                stack.append(node.right)
            if node.left:
                stack.append(node.left)
        
        return res
```

### 二叉树的中序遍历
> [Leetcode 94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

> 给定一个二叉树，返回它的 中序 遍历。

1、递归版本
```python
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        def _inorder(node):
            if node:
                _inorder(node.left)
                res.append(node.val)
                _inorder(node.right)

        res = []
        _inorder(root)
        return res
```

2、非递归版本
```python
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        if not root:
            return []

        res = []
        stack = []
        node = root
        
        while stack or node:
            if node:
                stack.append(node)
                node = node.left
            else:
                node = stack.pop()
                res.append(node.val)
                node = node.right
        
        return res
```

3、非递归版本，Morris中序遍历，空间复杂度$O(1)$

==TODO==


### 二叉树的后序遍历
> [Leetcode 145. 二叉树的后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)

> 给定一个二叉树，返回它的 后序 遍历。

1、递归版本
```python
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        def _postorder(node):
            if node:
                _postorder(node.left)
                _postorder(node.right)
                res.append(node.val)
        
        res = []
        _postorder(root)
        return res
```

2、非递归版本。推荐方法。
```python
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        if not root:
            return []
        
        res =  []
        stack = []
        node = root
        visited = None  # 用来标记已访问的结点

        while stack or node:
            if node:
                stack.append(node)
                node = node.left
            else:
                node = stack[-1]  
                # 关键：栈中最后一个结点，并检查该结点的右孩子是否为空或者刚被访问
                if node.right is None or node.right == visited: 
                    node = stack.pop()  # 访问该结点，并标记被访问
                    res.append(node.val)
                    visited = node
                    node = None
                else:
                    node = node.right  # 在右子树进行一次后序遍历

        return res
```

3、非递归版本。该方法遍历顺序与正常的后序遍历相反，最后需要把结果倒置一下。
```python
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        if root is None:
            return []

        stack = [root]
        res = []
        while stack:
            node = stack.pop()
            res.append(node.val)
            if node.left is not None:
                stack.append(node.left)
            if node.right is not None:
                stack.append(node.right)
                
        return res[::-1]
```


### 二叉树的层序遍历

1、普通层序遍历，返回一个列表。

方法1、不会往队列中添加空结点。
```python
class Solution:
    def levelOrder(self, root: TreeNode) -> List[int]:
        if not root:
            return []
        
        from collections import deque
        res = []
        queue = deque([root])  # 双向队列deque
        while queue:
            node = queue.popleft()
            res.append(node.val)
            if node.left: 
                queue.append(node.left)
            if node.right: 
                queue.append(node.right)
        return res
```

方法2、会往队列中添加空结点。
```python
class Solution:
    def levelOrder(self, root: TreeNode) -> List[int]:
        if not root:
            return []

        from collections import deque
        res = []
        queue = deque([root])  # 双向队列deque
        while queue:
            node = queue.popleft()
            if node:
                res.append(node.val)
                queue.append(node.left)
                queue.append(node.right)
            else:
                pass  # 可以进行一些操作
        return res
```

2、进阶层序遍历，每一层返回一个列表。
> [Leetcode 102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

> 给你一个二叉树，请你返回其按 层序遍历 得到的节点值。（即逐层地，从左到右访问所有节点）。

**解析**：

```python
class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        if not root:
            return []

        res = []
        queue = [root]
        while queue:
            cur_level_vals = []
            next_level_nodes = []
            for node in queue:
                cur_level_vals.append(node.val)
                if node.left: 
                    next_level_nodes.append(node.left)
                if node.right: 
                    next_level_nodes.append(node.right)
            res.append(cur_level_vals)
            queue = next_level_nodes
        return res
```


## N叉树遍历
### N叉树的前序遍历
> [Leetcode 589. N叉树的前序遍历](https://leetcode-cn.com/problems/n-ary-tree-preorder-traversal/)

> 给定一个 N 叉树，返回其节点值的前序遍历。

**解析**：

1、递归版本
```python
class Solution:
    def preorder(self, root: 'Node') -> List[int]:
        def _preorder(node):
            if node:
                res.append(node.val)
                for c in node.children:
                    _preorder(c)

        res = []
        _preorder(root)
        return res
```

2、非递归版本
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

### N叉树的后序遍历
> [Leetcode 590. N叉树的后序遍历](https://leetcode-cn.com/problems/n-ary-tree-postorder-traversal/)

> 给定一个 N 叉树，返回其节点值的后序遍历。

**解析**：

1、递归
```python
class Solution:
    def postorder(self, root: 'Node') -> List[int]:
        def _postorder(node):
            if node:
                for c in node.children:
                    _postorder(c)
                res.append(node.val)
        
        res = []
        _postorder(root)
        return res
```

2.非递归
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


### N叉树的层序遍历
1、普通层序遍历，返回一个列表。
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

2、进阶层序遍历，每一层返回一个列表。
> [Leetcode 429. N叉树的层序遍历](https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/)

> 给定一个 N 叉树，返回其节点值的层序遍历。 (即从左到右，逐层遍历)。

**解析**：

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


## 最近公共祖先
### 二叉树
> [Leetcode 236. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

> 给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

> 百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

**解析**：

```python
class Solution:
    def lowestCommonAncestor(self, root: TreeNode, p: TreeNode, q: TreeNode) -> TreeNode:
        if not root or root == p or root == q:
            return root
        
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        if not left and not right:
            return None
        elif not left:
            return right
        elif not right:
            return left
        return root
```

### 二叉搜索树
> [Leetcode 235. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

**解析**：

```python
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if not root:
            return None
        
        if root.val > p.val and root.val > q.val:
            return self.lowestCommonAncestor(root.left, p, q)
        elif root.val < p.val and root.val < q.val:
            return self.lowestCommonAncestor(root.right, p, q)

        return root
```

## 二叉树中从根到叶子的路径
### 模板
**解析**：
1. 利用层序遍历。
2. **关键**：访问到叶子结点时需要保存结果，也可以在`if`中添加指定的条件。

```python
class Solution:
    def binaryTreePaths(self, root: TreeNode) -> List[str]:
        if not root:
            return []
        
        from collections import deque  # 双向队列
        res = []
        node_queue = deque([root])
        path_queue = deque([[root.val]])
        while node_queue:
            node = node_queue.popleft()
            path = path_queue.popleft()
            if node.left:
                node_queue.append(node.left)
                path_queue.append(path + [node.left.val])
            if node.right:
                node_queue.append(node.right)
                path_queue.append(path + [node.right.val])
            if not node.left and not node.right:  # 叶子节点
                res.append(path)
        
        return res
```

### 二叉树的所有路径
> [Leetcode 257. 二叉树的所有路径](https://leetcode-cn.com/problems/binary-tree-paths/)

> 给定一个二叉树，返回所有从根节点到叶子节点的路径。

**解析**：

```python
class Solution:
    def binaryTreePaths(self, root: TreeNode) -> List[str]:
        if not root:
            return []
        
        from collections import deque
        res = []
        node_queue = deque([root])
        path_queue = deque([str(root.val)])
        while node_queue:
            node = node_queue.popleft()
            path = path_queue.popleft()
            if node.left:
                node_queue.append(node.left)
                path_queue.append(path + '->' + str(node.left.val))
            if node.right:
                node_queue.append(node.right)
                path_queue.append(path + '->' + str(node.right.val))
            if not node.left and not node.right:
                res.append(path)
        
        return res
```

### 路经总和
> [Leetcode 112. 路径总和](https://leetcode-cn.com/problems/path-sum/)

> 给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。

> 说明: 叶子节点是指没有子节点的节点。

**解析**：

1、使用模板
```python
class Solution:
    def hasPathSum(self, root: TreeNode, s: int) -> bool:
        if not root:
            return False
        
        from collections import deque
        res = []
        node_queue = deque([root])
        path_queue = deque([root.val])
        while node_queue:
            node = node_queue.popleft()
            path = path_queue.popleft()
            if node.left:
                node_queue.append(node.left)
                path_queue.append(path + node.left.val)
            if node.right:
                node_queue.append(node.right)
                path_queue.append(path + node.right.val)
            if not node.left and not node.right and path == s: # 叶子节点
                return True
        
        return False
```

2、递归
```python
class Solution:
    def hasPathSum(self, root: TreeNode, s: int) -> bool:
        if not root:
            return False

        if not root.left and not root.right:  # if reach a leaf
            return s - root.val == 0
        return self.hasPathSum(root.left, s - root.val) or self.hasPathSum(root.right, s - root.val)
```

### 路径总和 II
> [Leetcode 113. 路径总和 II](https://leetcode-cn.com/problems/path-sum-ii/)

> 给定一个二叉树和一个目标和，找到所有从根节点到叶子节点路径总和等于给定目标和的路径。

> 说明: 叶子节点是指没有子节点的节点。

**解析**：

1、使用模板
```python
class Solution:
    def pathSum(self, root: TreeNode, s: int) -> List[List[int]]:
        if not root:
            return []

        from collections import deque
        res = []
        node_queue = deque([root])
        path_queue = deque([[root.val]])
        while node_queue:
            node = node_queue.popleft()
            path = path_queue.popleft()
            if node.left:
                node_queue.append(node.left)
                path_queue.append(path + [node.left.val])
            if node.right:
                node_queue.append(node.right)
                path_queue.append(path + [node.right.val])
            if not node.left and not node.right and sum(path) == s:
                res.append(path)
        
        return res
```

2、递归
```python
class Solution:
    def pathSum(self, root: TreeNode, s: int) -> List[List[int]]:
        def get_all_path(node):
            if not node:
                return []
            elif not node.left and not node.right:
                return[[node.val]]

            left = get_all_path(node.left)
            right = get_all_path(node.right)
            return [[node.val] + path for path in left + right]
        
        res = get_all_path(root)
        return [path for path in res if sum(path) == s]
```

### 路径总和 III
> [Leetcode 437. 路径总和 III](https://leetcode-cn.com/problems/path-sum-iii/)

**解析**：参考 [递归模板](#Leetcode437)。


## 构造二叉树
### 使用数组构造二叉树
根据数组构造二叉树。

举例：
```
数组为：["1","2","3","null","null","4","5","null","null","null","null"]

对应的二叉树：
        1
       / \
      2   3
         / \
        4   5
```

**解析**：代码非Leetcode格式。

1、迭代
```python
def create_binary_tree(data):
    if not data:
        return None
        
    i = 0
    root = TreeNode(int(data[i]))
    queue = deque([root])
    while queue:
        node = queue.popleft()
        i += 1
        if data[i] != "null":
            node.left = TreeNode(int(data[i]))
            queue.append(node.left)
        i += 1
        if data[i] != "null":
            node.right = TreeNode(int(data[i]))
            queue.append(node.right)

    return root
```

2、递归
==TODO==


### 从前序与中序遍历序列构造二叉树
> [Leetcode 105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

> 根据一棵树的前序遍历与中序遍历构造二叉树。

> 注意:你可以假设树中没有重复的元素。

**解析**：

```python
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        def helper(preorder, inorder):
            if not preorder:
                return None
            if len(preorder) == 1:
                return TreeNode(preorder[0])

            index = inorder.index(preorder[0])
            root = TreeNode(preorder[0])
            root.left = helper(preorder[1:index + 1], inorder[:index])
            root.right = helper(preorder[index + 1:], inorder[index + 1:])
            return root

        return helper(preorder, inorder)
```

### 从中序与后序遍历序列构造二叉树
> [Leetcode 106. 从中序与后序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

> 根据一棵树的中序遍历与后序遍历构造二叉树。

> 注意:你可以假设树中没有重复的元素。

**解析**：

```python
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
        def helper(inorder, postorder):
            if not inorder:
                return None
            elif len(inorder) == 1:
                return TreeNode(inorder[0])

            root = TreeNode(postorder[-1])
            index = inorder.index(postorder[-1])
            n_right = len(inorder) - index - 1
            root.left = helper(inorder[:index], postorder[:index])
            root.right = helper(inorder[index + 1:], postorder[-1 - n_right:-1])
            return root

        return helper(inorder, postorder)
```

### 根据前序和后序遍历构造二叉树
> [Leetcode 889. 根据前序和后序遍历构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/)

> 返回与给定的前序和后序遍历匹配的任何二叉树。

> pre 和 post 遍历中的值是不同的正整数。长度相等。

> 每个输入保证至少有一个答案。如果有多个答案，可以返回其中一个。

**解析**：

```python
class Solution:
    def constructFromPrePost(self, pre: List[int], post: List[int]) -> TreeNode:
        def helper(pre, post):
            if not pre or not post or len(pre) != len(post):
                return None
            
            root = TreeNode(pre[0])
            if len(pre) == 1:
                return root
            
            index = post.index(pre[1])
            count = index + 1
            
            root.left = helper(pre[1 : count + 1], post[:count])
            root.right = helper(pre[count + 1:], post[count:-1])
            return root

        return helper(pre, post)
```


### 从先序遍历还原二叉树
> [Leetcode 1028. 从先序遍历还原二叉树](https://leetcode-cn.com/problems/recover-a-tree-from-preorder-traversal/)

> 我们从二叉树的根节点`root`开始进行深度优先搜索。

> 在遍历中的每个节点处，我们输出`D`条短划线（其中`D`是该节点的深度），然后输出该节点的值。（如果节点的深度为`D`，则其直接子节点的深度为`D + 1`。根节点的深度为`0`）。

> 如果节点只有一个子节点，那么保证该子节点为左子节点。

> 给出遍历输出`S`，还原树并返回其根节点`root`。

> 示例：

```
输入："1-2--3--4-5--6--7"

对应的二叉树：
         1
       /   \
      2     5
     / \   / \
    3   4 6   7
```

**解析**：

1、栈模拟。

> 参考 [Leetcode官方题解](https://leetcode-cn.com/problems/recover-a-tree-from-preorder-traversal/solution/cong-xian-xu-bian-li-huan-yuan-er-cha-shu-by-leetc/)

```python
class Solution:
    def recoverFromPreorder(self, S: str) -> TreeNode:
        stack = []
        pos = 0
        while pos < len(S):
            level = 0
            while S[pos] == '-':
                level += 1
                pos += 1
            value = 0
            while pos < len(S) and S[pos].isdigit():
                value = value * 10 + (ord(S[pos]) - ord('0'))
                pos += 1
            node = TreeNode(value)
            if level == len(stack):
                if stack:
                    stack[-1].left = node
            else:
                stack = stack[:level]
                stack[-1].right = node
            stack.append(node)
        return stack[0]
```

2、用哈希表，更容易理解。

> 参考 [Leetcode-Actonmic](https://leetcode-cn.com/problems/recover-a-tree-from-preorder-traversal/comments/450316)

```python
class Solution:
    def recoverFromPreorder(self, S: str) -> TreeNode:
        pos = 0
        saved = {0: None}
        while pos < len(S):
            level = 0
            while S[pos] == '-':
                level += 1
                pos += 1
            value = 0
            while pos < len(S) and S[pos].isdigit():
                value = value * 10 + (ord(S[pos]) - ord('0'))
                pos += 1
            node = TreeNode(value)
            if level == 0:
                saved[0] = node
            else:
                parent = saved[level - 1]
                if parent.left is None:
                    parent.left = node
                else:
                    parent.right = node
                saved[level] = node
        return saved[0]
```


### 先序遍历构造二叉搜索树
> [Leetcode 1008. 先序遍历构造二叉树](https://leetcode-cn.com/problems/construct-binary-search-tree-from-preorder-traversal/comments/)

> 返回与给定先序遍历 preorder 相匹配的二叉搜索树（binary search tree）的根结点。

**解析**：根据先序遍历构造二叉搜索树。

```python
class Solution:
    def bstFromPreorder(self, preorder: List[int]) -> TreeNode:
        def helper(preorder):
            if not preorder:
                return None
            if len(preorder) == 1:
                return TreeNode(preorder[0])

            root = TreeNode(preorder[0])
            begin, end = 1, 1
            while end < len(preorder) and preorder[end] < preorder[0]:
                end += 1
            
            root.left = helper(preorder[begin:end])
            root.right = helper(preorder[end:])
            return root
        
        return helper(preorder)
```


## 二叉树的一种递归模板
### 模板

```python
class Solution:
    def diameterOfBinaryTree(self, root: TreeNode) -> int:
        def helper(node):
            if not node:
                return # ...
            left = helper(node.left)
            right = helper(node.right)
            # process left and right
            return  # ...
        
        return helper(root)
```

### 平衡二叉树
> [Leetcode 110. 平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)

> 给定一个二叉树，判断它是否是高度平衡的二叉树。

> 本题中，一棵高度平衡二叉树定义为：一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过1。

**解析**：

```python
class Solution:
    def isBalanced(self, root: TreeNode) -> bool:
        def helper(node):
            if not node:
                return 0, True  # depth, is_balance
            left = helper(node.left)
            right = helper(node.right)
            depth = max(left[0], right[0]) + 1
            is_balance = left[1] and right[1] and abs(left[0] - right[0]) <= 1
            return depth, is_balance

        return helper(root)[1]
```

### 二叉树中的直径
> [Leetcode 543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

> 给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过也可能不穿过根结点。

**解析**：

```python
class Solution:
    def diameterOfBinaryTree(self, root: TreeNode) -> int:
        def helper(node):
            if not node:
                return 0, 0  # depth, diameter
            left = helper(node.left)
            right = helper(node.right)
            depth = max(left[0], right[0]) + 1
            diameter = max(left[0] + right[0], left[1], right[1])
            return depth, diameter
        
        return helper(root)[1]
```


### 二叉树的坡度
> [Leetcode 563. 二叉树的坡度](https://leetcode-cn.com/problems/binary-tree-tilt/)

> 给定一个二叉树，计算整个树的坡度。

> 一个树的节点的坡度定义即为，该节点左子树的结点之和和右子树结点之和的差的绝对值。空结点的的坡度是0。

> 整个树的坡度就是其所有节点的坡度之和。

**解析**：

```python
class Solution:
    def findTilt(self, root: TreeNode) -> int:
        def helper(node):
            if not node:
                return 0, 0 # sum, tilt
            left = helper(node.left)
            right = helper(node.right)
            sum_ = left[0] + right[0] + node.val
            tilt = left[1] + right[1] + abs(left[0] - right[0])
            return sum_, tilt
            
        return helper(root)[1]
```


### 二叉树中的最大路径和
> [Leetcode 124. 二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)

> 给定一个非空二叉树，返回其最大路径和。

> 本题中，路径被定义为一条从树中任意节点出发，达到任意节点的序列。该路径至少包含一个节点，且不一定经过根节点。

**解析**：如果当前结点的值小于0，那么路径可以不包含当前结点，当前路径的和最小为0。

```python
class Solution:
    def maxPathSum(self, root: TreeNode) -> int:
        def helper(node):
            if not node:
                return float("-inf"), 0  # res, the larger sum
            left = helper(node.left)
            right = helper(node.right)
            left_sum = max(0, left[1])
            right_sum = max(0, right[1])
            res = max(left[0], right[0], left_sum + node.val + right_sum)
            larger_sum = max(left_sum, right_sum) + node.val
            return res, larger_sum

        return helper(root)[0]
```

递归函数不返回目标变量，而是直接在内部修改，递归更简洁。
```python
class Solution:
    def maxPathSum(self, root: TreeNode) -> int:
        def helper(node):
            if not node:
                return 0
            nonlocal res
            left = helper(node.left)
            right = helper(node.right)
            left_sum = max(0, left)
            right_sum = max(0, right)
            res = max(res, left_sum + node.val + right_sum)
            return max(left_sum, right_sum) + node.val
        
        res = float('-inf')
        maxPath(root)
        return res
```


### <span id="Leetcode437">路径总和 III</span>
> [Leetcode 437. 路径总和 III](https://leetcode-cn.com/problems/path-sum-iii/)

> 给定一个二叉树，它的每个结点都存放着一个整数值。

> 找出路径和等于给定数值的路径总数。

> 路径不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

> 二叉树不超过1000个节点，且节点数值范围是 [-1000000,1000000] 的整数。

**解析**：

```python
class Solution:
    def pathSum(self, root: TreeNode, target: int) -> int:
        def helper(node, target):
            if not node:
                return [], 0  # path_sum, count
            left = helper(node.left, target)
            right = helper(node.right, target)
            path_sum = [ps + node.val for ps in left[0] + right[0]] + [node.val]
            count = left[1] + right[1]
            for ps in path_sum:
                if ps == target:
                    count += 1
            return path_sum, count
        
        return helper(root, target)[1]
```

递归函数不返回目标变量，而是直接在内部修改，递归更简洁。
```python
class Solution:
    def pathSum(self, root: TreeNode, target: int) -> int:
        def helper(node, target):
            if not node:
                return []  # path_sum
            nonlocal res
            left = helper(node.left, target)
            right = helper(node.right, target)
            path_sum = [ps + node.val for ps in left + right] + [node.val]
            for ps in path_sum:
                if ps == target:
                    res += 1
            return path_sum
        
        res = 0
        helper(root, target)
        return res
```


## 翻转二叉树
> [Leetcode 226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)

> 翻转一棵二叉树。

**解析**：

```python
class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        if not root:
            return None

        from collections import deque
        queue = deque([root])
        while queue:
            node = queue.popleft()
            node.left, node.right = node.right, node.left
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)

        return root
```


## 二叉树的序列化与反序列化
> [Leetcode 297. 二叉树的序列化与反序列化](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)

> 序列化是将一个数据结构或者对象转换为连续的比特位的操作，进而可以将转换后的数据存储在一个文件或者内存中，同时也可以通过网络传输到另一个计算机环境，采取相反方式重构得到原数据。

> 请设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。

> 举例：

```
二叉树：
        1
       / \
      2   3
         / \
        4   5
        
序列化形式为："[1,2,3,null,null,4,5,null,null,null,null]"。
```

**解析**：层序遍历。

```python
class Codec:
    def serialize(self, root):
        """Encodes a tree to a single string."""
        if not root:
            return "[]"
        
        from collections import deque
        queue = deque([root])
        values = []
        while queue:
            node = queue.popleft()
            if node:
                values.append(str(node.val))
                queue.append(node.left)
                queue.append(node.right)
            else:
                values.append("null")
        
        return "[" + ",".join(values) + "]"

    def deserialize(self, data):
        """Decodes your encoded data to tree."""
        if data == "[]":
            return None

        values = data[1:-1].split(",")
        i = 0
        root = TreeNode(int(values[i]))
        queue = deque([root])

        while queue:
            node = queue.popleft()
            i += 1
            if values[i] != "null":
                node.left = TreeNode(int(values[i]))
                queue.append(node.left)
            i += 1
            if values[i] != "null":
                node.right = TreeNode(int(values[i]))
                queue.append(node.right)

        return root
```


## 根据二叉树创建字符串
> [Leetcode 606. 根据二叉树创建字符串](https://leetcode-cn.com/problems/construct-string-from-binary-tree/)

> 你需要采用前序遍历的方式，将一个二叉树转换成一个由括号和整数组成的字符串。

> 空节点则用一对空括号 "()" 表示。而且你需要省略所有不影响字符串与原始二叉树之间的一对一映射关系的空括号对。

**解析**：递归比较简单。

```
class Solution:
    def tree2str(self, t: TreeNode) -> str:
        if not t:
            return ''
        elif not t.left and not t.right:
            return str(t.val)
        elif not t.left:
            return str(t.val) + '()(' + self.tree2str(t.right) + ')'
        elif not t.right:
            return str(t.val) + '(' + self.tree2str(t.left) + ')'
        return str(t.val) + '(' + self.tree2str(t.left) + ')(' + self.tree2str(t.right) + ')'
```


## 根据字符串创建二叉树
> [Leetcode 536. 从字符串生成二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-string/)

> [Lintcode 880. 字符串构造二叉树](https://www.lintcode.com/problem/construct-binary-tree-from-string)

> 你需要根据一个由括号和整数组成的字符串中构造一颗二叉树。

> 输入的整个字符串表示一个二叉树。它包含一个整数，以及其后跟随的0~2对括号。该整数表示根的值，而一对括号里的字符串代表一个具有相同结构的子二叉树。

> 示例：

```
输入: "-4(2(3)(1))(6(5))"
输入字符串对应的二叉树：
      -4
     /   \
    2     6
   / \   / 
  3   1 5 
```

**注意**：Leetcode需要会员，Lintcode不需要。

**解析**：迭代法和递归法。

1、迭代

> 参考 [LeetCode 536. Construct Binary Tree from String 从带括号字符串构建二叉树](https://segmentfault.com/a/1190000016808160)

```python
class Solution:
    def str2tree(self, s):
        if not s:
            return None
            
        stack = []
        i = 0
        while i < len(s):
            ch = s[i]
            if ch.isdigit() or ch == '-':
                begin = i
                while i + 1 < len(s) and s[i + 1].isdigit():
                    i += 1
                node = TreeNode(int(s[begin:i + 1]))
                if stack:
                    parent = stack[-1]
                    if parent.left is None:
                        parent.left = node
                    else:
                        parent.right = node
                stack.append(node)
            elif ch == ')':
                stack.pop()
            i += 1
        return stack[0]  
```


2、递归

> 参考 [Lintcode-xiongxiong](https://www.lintcode.com/problem/construct-binary-tree-from-string/note/187028)

```python
class Solution:
    def str2tree(self, s):
        if not s:
            return None
        left_begin = s.find('(')
        if left_begin == -1:
            return TreeNode(int(s))

        left_end = left_begin + 1
        num = 1
        while left_end < len(s):
            if s[left_end] == '(':
                num += 1
            elif s[left_end] == ')':
                num -= 1
                if num == 0:
                    break
            left_end += 1

        root = TreeNode(int(s[:left_begin]))
        root.left = self.str2tree(s[left_begin + 1: left_end])
        right_end = s.rfind(')')
        if right_end > left_end + 1:
            root.right = self.str2tree(s[left_end + 2: right_end])

        return root
```


## 将二叉搜索树转化为排序的双向链表
> [Leetcode 426. 将二叉搜索树转化为排序的双向链表](https://leetcode-cn.com/problems/convert-binary-search-tree-to-sorted-doubly-linked-list/)

> [剑指 Offer 36. 二叉搜索树与双向链表](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)

> 输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的循环双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。

**解析**：中序遍历，修改一下指针。

```python
class Solution:
    def treeToDoublyList(self, root: 'Node') -> 'Node':
        if root is None:
            return root

        pre = head = Node(None)
        stack = []
        node = root
        while node or stack:
            if node:
                stack.append(node)
                node = node.left
            else:
                node = stack.pop()
                node.left = pre
                pre.right = node
                pre = node
                node = node.right

        pre.right = head.right
        head.right.left = pre

        return head.right
```


## 有序链表转换二叉搜索树
> [Leetcode 109. 有序链表转换二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/)

> 给定一个单链表，其中的元素按升序排序，将其转换为**高度平衡的二叉搜索树**。

> 本题中，一个高度平衡二叉树是指一个二叉树每个节点的左右两个子树的高度差的绝对值不超过 1。

> 示例：

```
给定的有序链表： [-10, -3, 0, 5, 9],

一个可能的答案是：[0, -3, 9, -10, null, 5], 它可以表示下面这个高度平衡二叉搜索树：

      0
     / \
   -3   9
   /   /
 -10  5
```

**解析**：快慢指针+递归。

```python
class Solution:
    def sortedListToBST(self, head: ListNode) -> TreeNode:
        if not head:
            return None
        elif not head.next:
            return TreeNode(head.val)
        slow = fast = head
        pre = None
        while fast and fast.next:
            pre = slow
            slow = slow.next
            fast = fast.next.next
        pre.next = None
        root = TreeNode(slow.val)
        root.left = self.sortedListToBST(head)
        root.right = self.sortedListToBST(slow.next)
        return root
```


## 修剪二叉搜索树
> [Leetcode 669. 修剪二叉搜索树](https://leetcode-cn.com/problems/trim-a-binary-search-tree/)

> 给定一个二叉搜索树，同时给定最小边界L和最大边界R。通过修剪二叉搜索树，使得所有节点的值在[L, R]中 (R>=L) 。你可能需要改变树的根节点，所以结果应当返回修剪好的二叉搜索树的新的根节点。

**解析**：

```python
class Solution:
    def trimBST(self, root: TreeNode, L: int, R: int) -> TreeNode:
        if not root:
            return None
        elif root.val < L:
            return self.trimBST(root.right, L, R)
        elif root.val > R:
            return self.trimBST(root.left, L, R)

        root.left = self.trimBST(root.left, L, R)
        root.right = self.trimBST(root.right, L, R)
        return root
```


## 把二叉搜索树转换为累加树
> [Leetcode 538. 把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)

> 给定一个二叉搜索树（Binary Search Tree），把它转换成为累加树（Greater Tree)，使得每个节点的值是原来的节点值加上所有大于它的节点值之和。

**解析**：

```python
class Solution:
    def convertBST(self, root: TreeNode) -> TreeNode:
        def convert(node, num):
            if not node:
                return num
            node.val += convert(node.right, num)
            return convert(node.left, node.val)
             
        convert(root, 0)
        return root
```


## 删除二叉搜索树中的节点
> [Leetcode 450. 删除二叉搜索树中的节点](https://leetcode-cn.com/problems/delete-node-in-a-bst/)

> 给定一个二叉搜索树的根节点 root 和一个值 key，删除二叉搜索树中的 key 对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。

> 一般来说，删除节点可分为两个步骤：首先找到需要删除的节点；如果找到了，删除它。

> 说明： 要求算法时间复杂度为 O(h)，h 为树的高度。

**解析**：

1、递归
==TODO==

2、迭代
```python
class Solution:
    def deleteNode(self, root: TreeNode, key: int) -> TreeNode:
        def delete_node_by_copy(left_child, node):
            child = left_child
            parent = node
            while child.right:
                parent = child
                child = child.right
            if child == left_child:
                node.val = child.val
                parent.left = child.left
            else:
                node.val = child.val
                parent.right = child.left
        
        parent = None
        cur = root
        while cur and cur.val != key:
            parent = cur
            cur = cur.left if cur.val > key else cur.right
        
        if cur == None:
            return root
        if cur is root:
            if cur.left is None and cur.right is None:
                return None
            if cur.left and cur.right:
                delete_node_by_copy(cur.left, cur)
                return root
            if cur.left:
                return cur.left
            if cur.right:
                return cur.right
        if cur.left is None or cur.right is None:
            child = cur.left if cur.left else cur.right
            if parent.left is cur:
                parent.left = child
            else:
                parent.right = child
            return root
        
        delete_node_by_copy(cur.left, cur)
        return root
```


## 二叉搜索树迭代器
> [Leetcode 173. 二叉搜索树迭代器](https://leetcode-cn.com/problems/binary-search-tree-iterator/)

> 实现一个二叉搜索树迭代器。你将使用二叉搜索树的根节点初始化迭代器。

> 调用`next()`将返回二叉搜索树中的下一个最小的数。

**解析**：利用中序遍历。

```python
class BSTIterator:

    def __init__(self, root: TreeNode):
        self.stack = []
        self._inorder(root)
    
    def _inorder(self, node):
        cur = node
        while cur:
            self.stack.append(cur)
            cur = cur.left 

    def next(self) -> int:
        """
        @return the next smallest number
        """
        cur = self.stack.pop()
        val = cur.val
        cur = cur.right
        self._inorder(cur)
        return val

    def hasNext(self) -> bool:
        """
        @return whether we have a next smallest number
        """
        return len(self.stack) > 0
```


## 验证二叉树的前序序列化
> [Leetcode 331. 验证二叉树的前序序列化](https://leetcode-cn.com/problems/verify-preorder-serialization-of-a-binary-tree/)

> 序列化二叉树的一种方法是使用前序遍历。当我们遇到一个非空节点时，我们可以记录下这个节点的值。如果它是一个空节点，我们可以使用一个标记值记录，例如`#`。

> 例如，上面的二叉树可以被序列化为字符串"9,3,4,#,#,1,#,#,2,#,6,#,#"，其中 # 代表一个空节点。

> 给定一串以逗号分隔的序列，验证它是否是正确的二叉树的前序序列化。编写一个在不重构树的条件下的可行算法。

> 每个以逗号分隔的字符或为一个整数或为一个表示`null`指针的`'#'`。

**解析**：栈模拟。

> 参考 [Leetcode官方题解](https://leetcode-cn.com/problems/verify-preorder-serialization-of-a-binary-tree/solution/yan-zheng-er-cha-shu-de-qian-xu-xu-lie-hua-by-leet/)

```python
class Solution:
    def isValidSerialization(self, preorder: str) -> bool:
        slots = 1
        for node in preorder.split(','):
            slots -= 1
            if slots < 0:
                return False
            if node != '#':
                slots += 2
        
        return slots == 0
```


## 二叉树最大宽度
> [Leetcode 662. 二叉树最大宽度](https://leetcode-cn.com/problems/maximum-width-of-binary-tree/)

> 给定一个二叉树，编写一个函数来获取这个树的最大宽度。树的宽度是所有层中的最大宽度。这个二叉树与满二叉树（full binary tree）结构相同，但一些节点为空。

> 每一层的宽度被定义为两个端点（该层最左和最右的非空节点，两端点间的null节点也计入长度）之间的长度。

**解析**：广度优先搜索或深度优先搜索。

> 参考 [Leetcode官方题解](https://leetcode-cn.com/problems/maximum-width-of-binary-tree/solution/er-cha-shu-zui-da-kuan-du-by-leetcode/)

1、广度优先搜索
```python
class Solution:
    def widthOfBinaryTree(self, root: TreeNode) -> int:
        queue = [(root, 0, 0)]
        res = 0
        cur_depth = left = 0
        while queue:
            node, depth, pos = queue.pop(0)
            if node:
                queue.append((node.left, depth + 1, pos * 2))
                queue.append((node.right, depth + 1, pos * 2 + 1))
                if cur_depth != depth:
                    cur_depth = depth
                    left = pos
                res = max(pos - left + 1, res)
        return res
```

2、深度优先搜索
```python
class Solution(object):
    def widthOfBinaryTree(self, root):
        self.ans = 0
        left = {}
        def dfs(node, depth = 0, pos = 0):
            if node:
                left.setdefault(depth, pos)
                self.ans = max(self.ans, pos - left[depth] + 1)
                dfs(node.left, depth + 1, pos * 2)
                dfs(node.right, depth + 1, pos * 2 + 1)

        dfs(root)
        return self.ans
```


## 完全二叉树的结点个数
> [Leetcode 222. 完全二叉树的结点个数](https://leetcode-cn.com/problems/count-complete-tree-nodes/)

> 给出一个完全二叉树，求出该树的节点个数。

> 说明：完全二叉树的定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 h 层，则该层包含 1~2^h 个节点。

**解析**：遍历一次，或者使用二分搜索。

> 参考 [Leetcode官方题解](https://leetcode-cn.com/problems/count-complete-tree-nodes/solution/wan-quan-er-cha-shu-de-jie-dian-ge-shu-by-leetcode/)

1、遍历一次，递归
```python
class Solution:
    def countNodes(self, root: TreeNode) -> int:
        return 1 + self.countNodes(root.right) + self.countNodes(root.left) if root else 0
```

2、二分搜索
```python
class Solution:
    def compute_depth(self, node: TreeNode) -> int:
        d = 0
        while node.left:
            node = node.left
            d += 1
        return d

    def exists(self, idx: int, d: int, node: TreeNode) -> bool:
        left, right = 0, 2**d - 1
        for _ in range(d):
            mid = left + (right - left) // 2
            if idx > mid:
                node = node.right
                left = mid + 1
            else:
                node = node.left
                right = mid
        return node is not None
        
    def countNodes(self, root: TreeNode) -> int:
        if not root:
            return 0
        
        d = self.compute_depth(root)
        if d == 0:
            return 1
        
        left, right = 0, 2**d
        while left < right:
            mid = left + (right - left) // 2
            if self.exists(mid, d, root):
                left = mid + 1
            else:
                right = mid

        return (2**d - 1) + left
```


## 删除给定值的叶子节点
> [Leetcode 1325. 删除给定值的叶子节点](https://leetcode-cn.com/problems/delete-leaves-with-a-given-value/)

> 给你一棵以`root`为根的二叉树和一个整数`target`，请你删除所有值为`target`的叶子节点。

> 注意，一旦删除值为`target`的叶子节点，它的父节点就可能变成叶子节点；如果新叶子节点的值恰好也是`target`，那么这个节点也应该被删除。

> 也就是说，你需要重复此过程直到不能继续删除。

**解析**：

> 参考 [Leetcode官方题解](https://leetcode-cn.com/problems/delete-leaves-with-a-given-value/solution/shan-chu-gei-ding-zhi-de-xie-zi-jie-dian-by-leet-2/)

```python
class Solution:
    def removeLeafNodes(self, root: TreeNode, target: int) -> TreeNode:
        if not root:
            return None
        root.left = self.removeLeafNodes(root.left, target)
        root.right = self.removeLeafNodes(root.right, target)
        if not root.left and not root.right and root.val == target:
            return None
        return root
```


## 实现 Trie (前缀树)
> [Leetcode 208. 实现 Trie (前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)

> 实现一个 Trie (前缀树)，包含 insert, search, 和 startsWith 这三个操作。

> 示例:
```
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // 返回 true
trie.search("app");     // 返回 false
trie.startsWith("app"); // 返回 true
trie.insert("app");   
trie.search("app");     // 返回 true
```

> 说明:
> - 你可以假设所有的输入都是由小写字母 a-z 构成的。
> - 保证所有输入均为非空字符串。

**解析**：

> 参考 [Leetcode-a-fei-8](https://leetcode-cn.com/problems/implement-trie-prefix-tree/solution/shu-ju-jie-gou-she-ji-zhi-shi-xian-trie-qian-zhui-/)

1、使用结点类
```python
class TrieNode:
    def __init__(self):
        self.children = collections.defaultdict(TrieNode)
        self.is_word = False

class Trie:
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.root = TrieNode()

    def insert(self, word: str) -> None:
        """
        Inserts a word into the trie.
        """
        node = self.root
        for ch in word:
            node = node.children[ch]
        node.is_word = True

    def search(self, word: str) -> bool:
        """
        Returns if the word is in the trie.
        """
        node = self.root
        for ch in word:
            if ch not in node.children:
                return False
            else:
                node = node.children[ch]
        return node.is_word

    def startsWith(self, prefix: str) -> bool:
        """
        Returns if there is any word in the trie that starts with the given prefix.
        """
        node = self.root
        for ch in prefix:
            if ch not in node.children:
                return False
            else:
                node = node.children[ch]
        return True
```

2、使用单纯的字典
```python
class Trie:
    def __init__(self):
        self.root = {}

    def insert(self, word: str) -> None:
        node = self.root
        for ch in word:
            if ch not in node:
                node[ch] = {}
                node = node[ch]
            else:
                node = node[ch]
        node['is_word'] = True  # mark a word
                
    def search(self, word: str) -> bool:
        node = self.root
        for ch in word:
            if ch not in node:
                return False
            else:
                node = node[ch]
        return "is_word" in node        

    def startsWith(self, prefix: str) -> bool:
        node = self.root
        for ch in prefix:
            if ch not in node:
                return False
            else:
                node = node[ch]
        return True
```


## 恢复二叉搜索树
> [Leetcode 99. 恢复二叉搜索树](https://leetcode-cn.com/problems/recover-binary-search-tree/)

> 二叉搜索树中的两个节点被错误地交换。

> 请在不改变其结构的情况下，恢复这棵树。

**解析**：

```python
class Solution:
    def recoverTree(self, root: TreeNode) -> None:
        """
        Do not return anything, modify root in-place instead.
        """
        if not root:
            return
        stack = []
        node = root
        all_nodes = []
        while stack or node:
            if node:
                stack.append(node)
                node = node.left
            else:
                node = stack.pop()
                all_nodes.append(node)
                node = node.right
        
        wrong_nodes = []
        for i in range(1, len(all_nodes)):
            if all_nodes[i - 1].val > all_nodes[i].val:
                wrong_nodes.append(all_nodes[i -1])
                wrong_nodes.append(all_nodes[i])

        if len(wrong_nodes) == 2:
            n1, n2 = wrong_nodes
            n1.val, n2.val = n2.val, n1.val
        elif len(wrong_nodes) == 4:
            n1, n2 = wrong_nodes[0], wrong_nodes[3]
            n1.val, n2.val = n2.val, n1.val
```



# 图

## 图的表示

> 算法导论-第三版

图有多种表示方法，比较常用的表示法有邻接链表和邻接矩阵。

根据边的方向，图可以分为无向图和有向图；根据边的权重，图可以分为带权图和不带权图。

对于图$G=(V,E)$，其邻接链表表示由一个包含$|V|$条链表的数组$Adj$所构成，每个结点有一条链表。对于每个结点$u \in V$，邻接链表$Adj[u]$包含所有与结点$u$之间有边相连的结点$v$，即$Adj[u]$包含图$G$中所有与$u$邻接的结点（也可以说，该链表里包含指向这些结点的指针）。由于邻接链表代表的是图的边，在伪代码里，我们将数组$Adj$看做是图的一个属性，就如我们将边集合$E$看做是图的属性一样。因此，在伪代码里，我们将看到$G.Adj[u]$这样的表示。

对于邻接链表稍加修改，即可以用来表示**权重图**（带权图）。权重图是图中的每条边都带有一个相关的权重的图。该权重值通常有一个$w:E\rightarrow R$的权重函数给出。例如，设$G=(V,E)$为一个权重图，其权重函数为$w$，我们可以直接将边$(u,v)\in{E}$的权重值$w(u,v)$存放在结点$u$的邻接链表里。

对于邻接矩阵表示来说，我们通常会将图$G$中的结点编为$1,2,...,|V|$，这种编号是任意的。在进行此种编号之后，图$G$的邻接矩阵表示由一个$|V|×|V|$的矩阵$A=(a_{ij})$予以表示，该矩阵满足下述条件：

$$a_{ij=}\left\{\begin{matrix}
1, (i,j) \in E 
\\ 0, \;\;\;\;\, others
\end{matrix}\right.$$

与邻接链表表示法一样，邻接矩阵也可以用来是表示权重图。例如，如果$G=(V,E)$为一个权重图，其权重函数为$w$，则我们直接将边$(u,v)\in{E}$的权重$w(u,v)$存放在第$u$行第$v$列记录上。对于不存在的边，则在相应的行列记录上存放`NIL`（空值）。不过，对于许多问题来说，用`0`或者`∞`来**表示一条不存在的边可能更为便捷**。

下图给出一个无向图及其两种表示（来自算法导论）：

<img src="https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20201030132417.png?token=AE4F4YLXCON6LAWG5GHSDBC7TOSB2" alt="算法 无向图" style="zoom:80%;" />

下图给出一个有向图及其两种表示（来自算法导论）：

<img src="https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20201030132454.png?token=AE4F4YKHUPVKCZ74G6NOX327TOSEC" alt="算法 有向图" style="zoom:80%;" />

不管是无向图还是有向图，邻接链表表示法的存储空间需求均为$\Theta(V+E)$。不管一个图有多少条边，邻接矩阵的空间需求均为$\Theta(V^2)$。

虽然邻接链表表示法和邻接矩阵表示法在渐近意义下至少是一样空间有效的，但邻接矩阵表示法更为简单，因此在图规模比较小时，我们可能更倾向于使用邻接矩阵表示法。而且，对于无向图来说，邻接矩阵还有一个优势：每个记录只需要一位的空间。

在Python中，邻接矩阵可以用二维列表来表示，邻接链表可以用字典和列表来表示。

对于上面的无向图，邻接矩阵和邻接链表的表示如下所示：
```python
# 邻接矩阵
graph = [
    [0, 1, 0, 0, 1],
    [1, 0, 1, 1, 1],
    [0, 1, 0, 1, 0],
    [0, 1, 1, 0, 1],
    [1, 1, 0, 1, 0],
]
# 邻接链表
graph = {
    1: [2, 5], 
    2: [1, 3, 4, 5], 
    3: [2, 4], 
    4: [2, 3, 5], 
    5: [1, 2, 4],
}
```

对于上面的有向图，邻接矩阵和邻接链表的表示如下所示：
```python
# 邻接矩阵
graph = [
    [0, 1, 0, 1, 0, 0],
    [0, 0, 0, 0, 1, 0],
    [0, 0, 0, 0, 1, 1],
    [0, 1, 0, 0, 0, 0],
    [0, 0, 0, 1, 0, 0],
    [0, 0, 0, 0, 0, 1],
]
# 邻接链表
graph = {
    1: [2, 4], 
    2: [5], 
    3: [5, 6], 
    4: [2], 
    5: [4], 
    6: [6],
}
```


## 图的遍历
图的遍历，指的是从图中的任一顶点出发，对图中的所有顶点访问一次且只访问一次。图的遍历操作和树的遍历操作功能相似。图的遍历是图的一种基本操作，图的许多其它操作都是建立在遍历操作的基础之上。

由于图结构本身的复杂性，所以图的遍历操作也较复杂，主要表现在以下四个方面：
1. 在图结构中，没有一个自然的首结点，任意一个顶点都可作为第一个被访问的结点，所以在遍历前需要指定一个开始结点。
2. 在非连通图中，从一个顶点出发，只能够访问它所在的连通分量上的所有顶点，因此，还需考虑如何选取下一个出发点以访问图中其余的连通分量。
3. 在图结构中，如果有回路存在，那么一个顶点被访问之后，有可能沿回路又回到该顶点。
4. 在图结构中，一个顶点可以和其它多个顶点相连，当这样的顶点访问过后，存在如何选取下一个要访问的顶点的问题。

图的遍历方法目前有两种算法：广度（宽度）优先搜索和深度优先搜索。在广度优先搜索算法中，我们把源结点的数量限制为一个，而深度优先搜索则可以有多个源结点，因为广度优先搜索通常用来寻找从特定源结点出发的最短路径距离（及其相关的前驱子图），而深度优先搜索则常常作为另一个算法里的一个子程序。


### 广度优先搜索
给定图$G=(V,E)$和一个可以识别的源结点$s$，广度优先搜索对图中的边进行系统性的探索来发现可以从源结点到达的所有结点。该算法能够计算从源结点$s$到每个可到达的结点的距离（最少的边数），同时生成一棵“广度优先搜索树”。该树以源结点$s$为根结点，包含所有可从$s$到达的结点。对于每个可以从源结点$s$到达的结点$v$，在广度优先搜索树里从结点$s$到结点$v$的简单路径所对应的就是图$G$中从结点$s$到结点$v$的“最短路径”，即包含最少边数的路径。该算法既可以用于有向图，也可以用于无向图。

广度优先搜索之所以如此得名是因为该算法始终是将已发现结点和未发现结点之间的边界，沿其广度方向向外扩展。也就是说，广度优先搜索需要在发现距离源结点$s$为$k$的所有结点之后，才会发现距离源结点$s$为$k+1$的其他结点。

对于一个用邻接链表表示的图，广度优先搜索的代码如下所示：
```python
def BFS(graph, start):
    res = []
    visited = {node: False for node in graph.keys()}
    visited[start] = True
    distances = {start: 0}
    predecessors = {start: None}
    queue = [start]
    while len(queue) > 0:
        node = queue.pop(0)  # 最好使用双向队列collections.deque
        res.append(node)
        for adj_node in graph[node]:
            if not visited[adj_node]:
                visited[adj_node] = True
                distances[adj_node] = distances[node] + 1
                predecessors[adj_node] = node
                queue.append(adj_node)
    return res, distances, predecessors


if __name__ == "__main__":
    graph = {
        1: [2, 5],
        2: [1, 3, 4, 5],
        3: [2, 4],
        4: [2, 3, 5],
        5: [1, 2, 4],
    }
    res, distances, predecessors = BFS(graph, 1)
    print(res)  # [1, 2, 5, 3, 4]
    print(distances)  # {1: 0, 2: 1, 5: 1, 3: 2, 4: 2}
    print(predecessors)  # {1: None, 2: 1, 5: 1, 3: 2, 4: 2}
```

广度优先搜索的时间复杂度为$O(V+E)$。

### 深度优先搜索

深度优先搜索所使用的策略就像其名字所隐含的：只要可能，就在图中尽量“深入”。深度优先搜索总是对最近才发现的结点$v$的出发边进行探索，直到该结点的所有出发边都被发现为止。一旦结点$v$的所有出发边都被发现，搜索则“回溯”到$v$的前驱结点（$v$是经过该结点才被发现的），来搜索该前驱结点的出发边。该过程一直持续到从源结点可以到达的所有结点都被发现为止。如果还存在尚未发现的结点，则深度优先搜索将从这些未被发现的结点中任选一个作为新的源结点，并重复同样的搜索过程。

对于一个用邻接链表表示的图（无向图和有向图均可），深度优先搜索的代码如下所示：

```python
def DFS(graph, start, use_loop=True):
    """只适用于连通图，需要指定源结点"""
    res = []
    visited = {node: False for node in graph.keys()}
    if use_loop:
        DFS_loop(graph, start, visited, res)
    else:
        DFS_recursive(graph, start, visited, res)
    return res


def DFS2(graph, use_loop=True):
    """适用于连通图和非连通图，不需要指定源结点"""
    res = []
    visited = {node: False for node in graph.keys()}
    if use_loop:
        for node in graph.keys():  # 最终的遍历结果依赖于结点的访问顺序
            if not visited[node]:
                DFS_loop(graph, node, visited, res)
    else:
        for node in graph.keys():  # 最终的遍历结果依赖于结点的访问顺序
            if not visited[node]:
                DFS_recursive(graph, node, visited, res)
    return res


def DFS_loop(graph, start, visited, res):
    """深度优先搜索，循环方法，使用栈。
    对于只适用于连通图的DFS，可以不使用visited和res两个参数，
    而是在此函数内部定义二者，并在最后返回遍历结果res。
    """
    visited[start] = True
    stack = [start]
    while len(stack) > 0:
        node = stack.pop()
        res.append(node)
        for adj_node in reversed(graph[node]):  # 最终的遍历结果依赖于结点的访问顺序
            if not visited[adj_node]:
                visited[adj_node] = True
                stack.append(adj_node)


def DFS_recursive(graph, start, visited, res):
    """深度优先搜索，递归方法"""
    visited[start] = True
    res.append(start)
    for node in graph[start]:  # 最终的遍历结果依赖于结点的访问顺序
        if not visited[node]:
            DFS_recursive(graph, node, visited, res)


if __name__ == "__main__":
    graph = {
        1: [2, 5],
        2: [1, 3, 4, 5],
        3: [2, 4],
        4: [2, 3, 5],
        5: [1, 2, 4],
    }
    res = DFS(graph, 1, True)
    print(res)  # [1, 2, 3, 4, 5]
    res = DFS(graph, 1, False)
    print(res)  # [1, 2, 3, 4, 5]
    res = DFS2(graph, True)
    print(res)  # [1, 2, 3, 4, 5]
    res = DFS2(graph, False)
    print(res)  # [1, 2, 3, 4, 5]
```

深度优先搜索的时间复杂度为$O(V+E)$。

## 拓扑排序
对于一个有向无环图$G=(V,E)$来说，其拓扑排序是$G$中所有结点的一种线性排序，该次序满足以下条件：如果图$G$包含边$(u,v)$，则结点$u$在拓扑排序中处于结点$v$的前面（如果图包含环路，则不可能排出一个线性次序）。许多实际应用都需要使用有向无环图来指明事件的优先次序。

对于一个用邻接链表表示的有向无环图，可以使用深度优先搜索对其进行拓扑排序，如下所示：

```python
def DFS_recursive(graph, start, visited, res):
    """深度优先搜索，递归方法"""
    visited[start] = True
    for node in graph[start]:
        if not visited[node]:
            DFS_recursive(graph, node, visited, res)
    res.insert(0, start)


def topological_sort(graph):
    res = []
    visited = {node: False for node in graph.keys()}
    for node in range(len(graph)):
        if not visited[node]:
            DFS_recursive(graph, node, visited, res)
    return res


if __name__ == "__main__":
    clothes = {
        0: "underwear",
        1: "pants",
        2: "belt",
        3: "suit",
        4: "shoe",
        5: "socks",
        6: "shirt",
        7: "tie",
        8: "watch",
    }
    graph = {
        0: [1, 4],
        1: [2, 4],
        2: [3],
        3: [],
        4: [],
        5: [4],
        6: [2, 7],
        7: [3],
        8: [],
    }
    res = topological_sort(graph)
    for ele in res:
        print(str(ele) + "-" + clothes[ele], end=" ")
    # 8-watch 6-shirt 7-tie 5-socks 0-underwear 1-pants 4-shoe 2-belt 3-suit
```

经过拓扑排序后的结点次序，与结点的完成时间恰好相反，所以在`DFS_recursive`的最后一行保存结果。拓扑排序的时间复杂度为$O(V+E)$，因为深度优先搜索的运行时间为$O(V+E)$。

在有向无环图上执行拓扑排序还有一种方法，就是重复寻找入度为0的结点，输入该结点，将该结点及从其发出的边从图中删除。


## 最小生成树
对于一个带权重的连通无向图$G=(V,E)$和权重函数$w:E \rightarrow R$，该权重函数将每条边映射到实数值的权重上。最小生成树（Minimum Spanning Tree，MST）问题是指，找到一个无环子集$T\subseteq E$，能够将所有的结点连接起来，又具有最小的权重。

解决最小生成树问题有两种算法：Kruskal算法和Prim算法。这两种算法都是贪心算法。贪心算法通常在每一步有多个可能的选择，并推荐选择在当前看来最好的选择。这种策略一般并不能保证找到一个全局最优的解决方案。但是，对于最小生成树问题来说，可以证明，Kruskal算法和Prim算法使用的贪心策略确实能够找到一棵权重最小的生成树。

### Kruskal

对于一个带权重的连通无向图$G=(V,E)$，Kruskal算法把图中的每一个结点看作一棵树，所以图中的所有结点可以组成一个森林。该算法按照边的权重大小依次进行考虑，如果一条边可以将两棵不同的树连接起来，它就被加入到森林中，从而完成对两棵树的合并。

在Kruskal算法的实现中，使用了一种叫做并查集的数据结构，其作用是用来维护几个不相交的元素集合。在该算法中，每个集合代表当前森林中的一棵树。

对于一个用邻接链表表示的带权重的连通无向图，Kruskal算法的实现如下所示：

```python
def mst_kruskal(graph, weights):
    edges = []
    for edge, weight in weights.items():
        if edge[0] < edge[1]:
            edges.append((edge, weight))
    edges.sort(key=lambda x: x[1])
    parents = {node: node for node in graph}

    def find_parent(node):
        if node != parents[node]:
            parents[node] = find_parent(parents[node])
        return parents[node]

    minimum_cost = 0
    minimum_spanning_tree = []

    for edge in edges:
        parent_from_node = find_parent(edge[0][0])
        parent_to_node = find_parent(edge[0][1])
        if parent_from_node != parent_to_node:
            minimum_cost += edge[1]
            minimum_spanning_tree.append(edge)
            parents[parent_from_node] = parent_to_node

    return minimum_spanning_tree, minimum_cost


if __name__ == "__main__":
    # 算法导论图23-4
    graph = {
        "a": ["b", "h"],
        "b": ["a", "c", "h"],
        "c": ["b", "d", "f", "i"],
        "d": ["c", "e", "f"],
        "e": ["d", "f"],
        "f": ["c", "d", "e", "g"],
        "g": ["f", "h", "i"],
        "h": ["a", "b", "g", "i"],
        "i": ["c", "g", "h"],
    }
    weights = {
        ("a", "b"): 4, ("a", "h"): 8,
        ("b", "a"): 4, ("b", "c"): 8, ("b", "h"): 11,
        ("c", "b"): 8, ("c", "d"): 7, ("c", "f"): 4, ("c", "i"): 2,
        ("d", "c"): 7, ("d", "e"): 9, ("d", "f"): 14,
        ("e", "d"): 9, ("e", "f"): 10,
        ("f", "c"): 4, ("f", "d"): 14, ("f", "e"): 10, ("f", "g"): 2,
        ("g", "f"): 2, ("g", "h"): 1, ("g", "i"): 6,
        ("h", "a"): 8, ("h", "b"): 11, ("h", "g"): 1, ("h", "i"): 7,
        ("i", "c"): 2, ("i", "g"): 6, ("i", "h"): 7,
    }
    minimum_spanning_tree, minimum_cost = mst_kruskal(graph, weights)
    print(minimum_spanning_tree)
    print(minimum_cost)
    # [(('g', 'h'), 1), (('c', 'i'), 2), (('f', 'g'), 2), (('a', 'b'), 4), (('c', 'f'), 4), (('c', 'd'), 7), (('a', 'h'), 8), (('d', 'e'), 9)]
    # 37
```

Kruskal算法的运行时间依赖于不相交集合数据结构的实现方式。如果使用不相交集合森林（并查集）实现，Kruskal算法的总运行时间为$O(E\lg{E})$。

### Prim

对于一个带权重的连通无向图$G=(V,E)$，Prim算法从图中任意一个结点$r$开始建立最小生成树，这棵树一直长大到覆盖$V$中的所有结点为止。与Kruskal算法不同，该算法始终保持只有一棵树，每一步选择与当前的树相邻的权重最小的一条边（也就是选择与当前的树最近的一个结点），加入到这棵树中。当算法终止时，所有已选择的边形成一棵最小生成树。本策略也属于贪心策略，因为每一步所加入的边都必须是使树的总权重增加量最小的边。

在Prim算法的实现中，需要使用最小优先队列来快速选择一条新的边，以便加入到已选择的边构成的树中。所以，在算法的执行过程中，对于不在当前的树中的每一个结点，需要记录其和树中结点的所有边中最小边的权重。

对于一个用邻接链表表示的带权重的连通无向图，Prim算法的实现如下所示：

```python
class MinHeap:
    def __init__(self, nodes, keys):
        """
        :param nodes: 保存结点元素
        :param keys: 保存结点的关键值
        item_pos: 保存结点元素在堆中的下标
        """
        self.heap = nodes
        self.size = len(nodes)
        self.keys = keys
        self.item_pos = {item: i for i, item in enumerate(self.heap)}
        self._heapify()

    def __len__(self):
        return self.size

    def _siftup(self, pos):
        """当前元素上筛"""
        old_item = self.heap[pos]
        while pos > 0:
            parent_pos = (pos - 1) >> 1
            parent_item = self.heap[parent_pos]
            if self.keys[old_item] < self.keys[parent_item]:
                self.heap[pos] = parent_item
                self.item_pos[parent_item] = pos
                pos = parent_pos
            else:
                break
        self.heap[pos] = old_item
        self.item_pos[old_item] = pos

    def _siftdown(self, pos):
        """当前元素下筛"""
        old_item = self.heap[pos]
        child_pos = 2 * pos + 1  # left child position
        while child_pos < self.size:
            child_item = self.heap[child_pos]
            right_child_pos = child_pos + 1
            right_child_item = self.heap[right_child_pos]
            if right_child_pos < self.size and \
                    self.keys[child_item] > self.keys[right_child_item]:
                child_pos = right_child_pos
                child_item = self.heap[child_pos]
            if self.keys[old_item] > self.keys[child_item]:
                self.heap[pos] = child_item
                self.item_pos[child_item] = pos
                pos = child_pos
                child_pos = 2 * pos + 1  # 更新循环判断条件
            else:
                break
        self.heap[pos] = old_item
        self.item_pos[old_item] = pos

    def _heapify(self):
        for i in reversed(range(self.size // 2)):
            self._siftdown(i)

    def extract_min(self):
        old_item = self.heap[0]
        self.heap[0] = self.heap[self.size - 1]
        self.item_pos[self.heap[0]] = 0
        self.heap[self.size - 1] = old_item
        self.item_pos[old_item] = self.size - 1
        self.size -= 1
        self._siftdown(0)
        return old_item

    def decrease_key(self, item):
        pos = self.item_pos[item]
        self._siftup(pos)

    def contains(self, item):
        return self.item_pos[item] < self.size


def mst_prim(graph, weights, start):
    keys = {}  # 保存每个结点的关键值（与树的最小距离）
    predecessors = {}  # 保存每个结点在最小生成树中的父结点
    for node in graph.keys():
        keys[node] = float("INF")
        predecessors[node] = None
    keys[start] = 0

    priority_queue = MinHeap(list(graph.keys()), keys)
    minimum_spanning_tree = []
    minimum_cost = 0

    while len(priority_queue) > 0:
        node = priority_queue.extract_min()
        minimum_spanning_tree.append((node, predecessors[node]))
        edge = (node, predecessors[node])
        if edge in weights:
            minimum_cost += weights[edge]
        for adj_node in graph[node]:
            if priority_queue.contains(adj_node) and weights[(node, adj_node)] < keys[adj_node]:
                predecessors[adj_node] = node
                keys[adj_node] = weights[(node, adj_node)]
                priority_queue.decrease_key(adj_node)

    return minimum_spanning_tree, minimum_cost


if __name__ == "__main__":
    # 算法导论图23-5
    graph = {
        "a": ["b", "h"],
        "b": ["a", "c", "h"],
        "c": ["b", "d", "f", "i"],
        "d": ["c", "e", "f"],
        "e": ["d", "f"],
        "f": ["c", "d", "e", "g"],
        "g": ["f", "h", "i"],
        "h": ["a", "b", "g", "i"],
        "i": ["c", "g", "h"],
    }
    weights = {
        ("a", "b"): 4, ("a", "h"): 8,
        ("b", "a"): 4, ("b", "c"): 8, ("b", "h"): 11,
        ("c", "b"): 8, ("c", "d"): 7, ("c", "f"): 4, ("c", "i"): 2,
        ("d", "c"): 7, ("d", "e"): 9, ("d", "f"): 14,
        ("e", "d"): 9, ("e", "f"): 10,
        ("f", "c"): 4, ("f", "d"): 14, ("f", "e"): 10, ("f", "g"): 2,
        ("g", "f"): 2, ("g", "h"): 1, ("g", "i"): 6,
        ("h", "a"): 8, ("h", "b"): 11, ("h", "g"): 1, ("h", "i"): 7,
        ("i", "c"): 2, ("i", "g"): 6, ("i", "h"): 7,
    }
    minimum_spanning_tree, minimum_cost = mst_prim(graph, weights, "a")
    print(minimum_spanning_tree)
    print(minimum_cost)
    # [('a', None), ('b', 'a'), ('h', 'a'), ('g', 'h'), ('f', 'g'), ('c', 'f'), ('i', 'c'), ('d', 'c'), ('e', 'd')]
    # 37
```

Prim算法的运行时间取决于最小优先队列的实现方式。如果最小优先队列使用二叉最小优先队列（最小堆），该算法的时间复杂度为$O(E\lg{V})$。从渐进意义上来说，它与Kruskal算法的运行时间相同。如果使用斐波那契堆来实现最小优先队列，则Prim算法的运行时间将改进到$O(E+V\lg{V})$。

## 最短路径

给定一个带权重的有向图$G=(V,E)$和权重函数$w:E \rightarrow R$，该权重函数将每条边映射到实数值的权重上。最短路径问题包括单源最短路径和所有结点对的最短路径问题。

- 单源最短路径问题是指，给定源结点$s \in V$，找到该结点到每个结点$v \in V$的最短路径。
- 所有结点对的最短路径问题是指，对于所有的结点对$u,v \in V$，找到从结点$u$到结点$v$的最短路径。

最短路径的一个重要性质是：两个结点之间的一条最短路径包含着其他的最短路径。简单来说，一条最短路径的所有子路径都是最短路径。

某些单源最短路径问题可能包含权重为负值的边。但只要图$G=(V,E)$中不包含从源结点$s$可以到达的权重为负值的环路，则对于所有的结点$v \in V$，最短路径权重$\delta(s,v)$都有精确定义，是可以计算的，即使其取值是负数。

单源最短路径的常用算法有Bellman-Ford算法和DijkiStra算法。DijkiStra算法假设输入图的所有的边权重为非负值。如果图中有权重为负值的边，则无法应用DijkiStra算法。而Bellman-Ford算法允许图中含有负权重的边，但不能有可以从源结点到达的权重为负值的环路。在通常情况下，如果存在一条权重为负值的环路，Bellman-Ford算法可以侦测并报告其存在。

所有结点对的最短路径问题可以通过运行$|V|$次单源最短路径算法来解决，每一次使用一个不同的结点作为源结点。如果图中所有的边的权重为非负值，可以使用Dijkistra算法。如果图中有权重为负值的边，那么只能使用效率更低的Bellman-Ford算法。

但是，所有结点对的最短路径问题有更好的算法，Floyd-Warshall算法和Johnson算法。

### Bellman-Ford

Bellman-Ford算法解决的是一般情况下的单源最短路径问题，允许图中边的权重为负值。该算法比较简单，并且还能够侦测是否存在从源结点可以到达的权重为负值的环路。

Bellman-Ford算法通过对边进行松弛操作来渐进地降低从源结点到每个结点的最短路径的估计距离，直到得到最终的最短路径。当且仅当输入图不包含可以从源结点到达的权重为负值的环路，该算法返回`TRUE`值，反之返回`FALSE`值。

对于一个用邻接链表表示的带权重的有向图，Bellman-Ford算法的实现如下所示：

```python
def intialize_single_source(graph, start):
    distances = {}  # 结点的关键值
    predecessors = {}  # 前驱
    for node in graph.keys():
        distances[node] = float("inf")
        predecessors[node] = None
    distances[start] = 0
    return distances, predecessors


def relax(u, v, w, distances, predecessors):
    if distances[v] > distances[u] + w[(u, v)]:
        distances[v] = distances[u] + w[(u, v)]
        predecessors[v] = u


def bellman_ford(graph, weights, start):
    distances, predecessors = intialize_single_source(graph, start)

    for _ in range(len(graph.keys())):
        for edge in weights.keys():
            u, v = edge[0], edge[1]
            relax(u, v, weights, distances, predecessors)

    for edge in weights.keys():
        u, v = edge[0], edge[1]
        if distances[v] > distances[u] + weights[edge]:
            return False, None, None  # 说明存在权重为负值的环路

    return True, distances, predecessors


if __name__ == "__main__":
    # 算法导论 图24-4
    graph = {
        "s": ["t", "y"],
        "t": ["x", "y", "z"],
        "x": ["t"],
        "y": ["x", "z"],
        "z": ["s", "x"],
    }
    weights = {
        ("s", "t"): 6, ("s", "y"): 7,
        ("t", "x"): 5, ("t", "y"): 8, ("t", "z"): -4,
        ("x", "t"): -2,
        ("y", "x"): -3, ("y", "z"): 9,
        ("z", "s"): 2, ("z", "x"): 7,
    }
    flag, distances, predecessors = bellman_ford(graph, weights, "s")
    print(flag)
    print(distances)
    print(predecessors)
    # True
    # {'s': 0, 't': 2, 'x': 4, 'y': 7, 'z': -2}
    # {'s': None, 't': 'x', 'x': 'y', 'y': 's', 'z': 't'}
```

Bellman-Ford算法的运行时间为$O(VE)$。

### Dijkistra

Dijkistra算法解决的是带权重的有向图上的单源最短路径问题，该算法要求所有边的权重都为非负值。

给定一个带权重的有向图$G=(V,E)$，该算法在运行过程中维持的关键信息是一组已访问的结点集合$S$，从源结点到该集合中每个结点之间的最短路径已经被被找到。算法重复从结点集$V-S$中选择最短路径最小的结点$u$，将结点$u$加入到集合$S$，然后对所有从$u$发出的边进行松弛。

在Dijkistra算法的实现中，需要使用最小优先队列来保存结点集$V-S$，根据每个结点的关键值，快速选择一个“最近”的结点。

因为Dijkistra算法总是选择集合$V-S$中“最近”的结点来加入到集合$S$中，该算法使用的是贪心策略。虽然贪心策略并不总是能获得最优的结果，但是可以证明，使用贪心策略的Dijkistra算法确实能够计算出最短路径。

对于一个用邻接链表表示的带权重的有向图，Dijkistra算法的实现如下所示：

```python
class MinHeap:
    pass  # 与Prim算法中的相同

def intialize_single_source(graph, start):
    distances = {}  # 结点的关键值
    predecessors = {}  # 前驱
    for node in graph.keys():
        distances[node] = float("inf")
        predecessors[node] = None
    distances[start] = 0
    return distances, predecessors


def relax(u, v, w, distances, predecessors, priority_queue):
    if distances[v] > distances[u] + w[(u, v)]:
        distances[v] = distances[u] + w[(u, v)]
        predecessors[v] = u
        priority_queue.decrease_key(v)


def dijkistra(graph, weights, start):
    distances, predecessors = intialize_single_source(graph, start)
    visited = []
    priority_queue = MinHeap(list(graph.keys()), distances)

    while len(priority_queue) > 0:
        node = priority_queue.extract_min()
        visited.append(node)
        for adj_node in graph[node]:
            relax(node, adj_node, weights, distances, predecessors, priority_queue)
    return visited, distances, predecessors


if __name__ == "__main__":
    # 算法导论 图24-6
    graph = {
        "s": ["t", "y"],
        "t": ["x", "y"],
        "x": ["z"],
        "y": ["t", "x", "z"],
        "z": ["s", "x"],
    }
    weights = {
        ("s", "t"): 10, ("s", "y"): 5,
        ("t", "x"): 1, ("t", "y"): 2,
        ("x", "z"): 4,
        ("y", "t"): 3, ("y", "x"): 9, ("y", "z"): 2,
        ("z", "s"): 7, ("z", "x"): 6,
    }
    visited, distances, predecessors = dijkistra(graph, weights, "s")
    print(visited)
    print(distances)
    print(predecessors)
    # ['s', 'y', 'z', 't', 'x']
    # {'s': 0, 't': 8, 'x': 9, 'y': 5, 'z': 7}
    # {'s': None, 't': 'y', 'x': 't', 'y': 's', 'z': 'y'}
```

Dijkistra算法的运行时间依赖于最小优先队列的实现。如果使用普通的数组来实现最小优先队列，意味着每次都需要遍历所有的未访问的结点才能找到“最近”的结点，算法的总运行时间为$O(V^2)$。如果使用二叉最小堆来实现最小优先队列，算法的总运行时间为$O((V+E)\lg{V})$。如果使用斐波那契堆来实现最小优先队列，算法的总运行时间可以改善到$O(V\lg{V}+E)$。

### Floyd-Warshall

Floyd-Warshall算法容易理解，其运行时间为$O(V^3)$。与前面的假设一样，负权重的边可以存在，但是不能存在权重为负值的环路。

Floyd算法通常使用邻接矩阵来表示一个图。为了方便起见，对于一个有$n$个结点的带权重的有向图$G=(V,E)$，结点的编号为$1,2,...,|V|=n$，可以使用一个大小为$n \times n$的矩阵$W$来表示。也就是说，$W=(w_{ij})$，其中$$w_{ij}= \left\{\begin{matrix}
0                   & \text{若}i=j\\ 
\text{有向边}(i,j)\text{的权重} & \text{若} i\neq j且(i,j) \in E\\
\infty                          & \text{若}i\neq j且(i,j) \notin E
\end{matrix}\right.$$

图中允许存在负权重的边，但是不包括权重为负值的环路。

对于一个用邻接矩阵表示的带权重的有向图，Floyd-Warshall算法的实现如下所示：

```python
INF = float("INF")


def floyd_warshall(graph):
    nodes = list(range(1, len(graph) + 1))
    predecessors = []
    for i, row in enumerate(graph):
        pres = []
        for j, ele in enumerate(row):
            if i == j:
                pres.append(None)
            elif ele is INF:
                pres.append(None)
            else:
                pres.append(nodes[i])
        predecessors.append(pres)

    distances = [row.copy() for row in graph]
    n = len(graph)
    for k in range(n):
        for i in range(n):
            for j in range(n):
                tmp = distances[i][k] + distances[k][j]
                if distances[i][j] > tmp:
                    distances[i][j] = tmp
                    predecessors[i][j] = predecessors[k][j]

    return distances, predecessors


if __name__ == "__main__":
    # 算法导论图25-4
    graph = [
        [0, 3, 8, INF, -4],
        [INF, 0, INF, 1, 7],
        [INF, 4, 0, INF, INF],
        [2, INF, -5, 0, INF],
        [INF, INF, INF, 6, 0],
    ]
    distances, predecessors = floyd_warshall(graph)
    print(distances)
    print(predecessors)
    # [[0, 1, -3, 2, -4], [3, 0, -4, 1, -1], [7, 4, 0, 5, 3], [2, -1, -5, 0, -2], [8, 5, 1, 6, 0]]
    # [[None, 3, 4, 5, 1], [4, None, 4, 2, 1], [4, 3, None, 2, 1], [4, 3, 4, None, 1], [4, 3, 4, 5, None]
```


## 访问所有节点的最短路径
> [Leetcode 847. 访问所有节点的最短路径](https://leetcode-cn.com/problems/shortest-path-visiting-all-nodes/)

> 给出`graph`为有 N 个节点（编号为`0, 1, 2, ..., N-1`）的无向连通图。
>
> `graph.length = N`，且只有节点`i`和`j`连通时，`j != i`在列表`graph[i]`中恰好出现一次。
>
> 返回能够访问所有节点的最短路径的长度。你可以在任一节点开始和停止，也可以多次重访节点，并且可以重用边。

**解析**：==TODO==



# 排序

## 经典排序算法

> [Leetcode 912. 排序数组](https://leetcode-cn.com/problems/sort-an-array/)

> 给你一个整数数组 nums，请你将该数组升序排列。

代码格式非Leetcode！

一、冒泡排序

```python
def bubble_sort(A):
    for i in range(len(A)):
        for j in range(0, len(A) - i - 1):
            if A[j] > A[j + 1]:
                A[j], A[j + 1] = A[j + 1], A[j]
```

二、插入排序

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

三、选择排序

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

四、归并排序

1、返回数组法。便于记忆。

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
    
# res = merge_sort(A, 0, len(A) - 1)
```

2、原地操作法。函数参数稍微复杂。

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

五、快速排序

介绍三种写法的快速排序。三种写法的`quick_sort`函数相同，在此给出。

推荐使用**填坑法**，比较直观，便于记忆。

```python
def quick_sort(A, left, right):
    if left < right:
        p = partition(A, left, right)
        quick_sort(A, left, p - 1)
        quick_sort(A, p + 1, right)
        
# quick_sort(A, 0, len(A) - 1)
```

1、填坑法。第一个元素当作轴元素。注意先从右往左比较，大于等于号；再从左往右比较，小于号。

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

2、交换法。第一个元素当作轴元素。注意先从右往左比较，大于等于号；再从左往右比较，小于等于号。

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

3、顺序遍历法。算法导论中的写法，选择最后一个元素作为轴元素。

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

七、堆排序

> [Python实现 《算法导论 第三版》中的算法 第6章 堆排序](https://blog.csdn.net/shengchaohua163/article/details/83038413)

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

八、线性时间排序

> [Python实现 《算法导论 第三版》中的算法 第8章 线性时间排序](https://blog.csdn.net/shengchaohua163/article/details/83444059)

比较排序的时间复杂度下界是$O(n\lg n)$。归并排序、堆排序和快速排序能在$O(n\lg n)$时间内排序`n`个数。归并排序和堆排序在最坏情况下能够达到该时间，快速排序在平均情况达到该时间（快速排序最坏情况下是$O(n^2)$）。

该文介绍了三种线性时间复杂度的排序算法：计数排序、基数排序和桶排序。



## 把数组排成最小的数

> [Leetcode 剑指 Offer 45. 把数组排成最小的数](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)

> 输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。

解析：字典顺序。

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

## 合并区间

> [Leetcode 56. 合并区间](https://leetcode-cn.com/problems/merge-intervals/)

> 给出一个区间的集合，请合并所有重叠的区间。

**解析**：排序即可。

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

## 根据身高重建队列

> [Leetcode 406. 根据身高重建队列](https://leetcode-cn.com/problems/queue-reconstruction-by-height/)

> 假设有打乱顺序的一群人站成一个队列。 每个人由一个整数对(h, k)表示，其中h是这个人的身高，k是排在这个人前面且身高大于或等于h的人数。 编写一个算法来重建这个队列。
>
> 注意：总人数少于1100人。
>
> 示例：
>
> ```
> 输入:
> [[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]
> 
> 输出:
> [[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]
> ```

**解析**：按身高降序、人数升序排序。

> 参考 [Leetcode 官方题解](https://leetcode-cn.com/problems/queue-reconstruction-by-height/solution/gen-ju-shen-gao-zhong-jian-dui-lie-by-leetcode/)

```python
class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        people.sort(key = lambda x: (-x[0], x[1]))
        res = []
        for p in people:
            res.insert(p[1], p)
        return res
````


## 数组中的逆序对

> [Leetcode 剑指 Offer 51. 数组中的逆序对](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

> 在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。
>
> 示例：
>
> ```
> 输入: [7,5,6,4]
> 输出: 5
> ```

**解析**：使用归并排序。

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


## 翻转对

> [Leetcode 493. 翻转对](https://leetcode-cn.com/problems/reverse-pairs/)

> 给定一个数组`nums`，如果`i < j`且`nums[i] > 2 * nums[j]`我们就将`(i, j)`称作一个重要翻转对。
>
> 你需要返回给定数组中的重要翻转对的数量。
>
> 示例1：
>
> ```
> 输入: [1,3,2,3,1]
> 输出: 2
> ```

**解析**：

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


## 计算右侧小于当前元素的个数

> [Leetcode 315. 计算右侧小于当前元素的个数](https://leetcode-cn.com/problems/count-of-smaller-numbers-after-self/)

> 给定一个整数数组 nums，按要求返回一个新数组counts。数组counts有该性质：`counts[i] `的值是`nums[i]`右侧小于`nums[i]`的元素的数量。
>
> 示例：
>
> ```
> 输入：nums = [5,2,6,1]
> 输出：[2,1,1,0] 
> 解释：
> 5 的右侧有 2 个更小的元素 (2 和 1)
> 2 的右侧有 1 个更小的元素 (1)
> 6 的右侧有 1 个更小的元素 (1)
> 1 的右侧有 0 个更小的元素
> ```

**解析**：二分查找、归并排序、树状数组等。

1、二分查找

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

2、归并排序

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

3、树状数组

==TODO==


## 区间和的个数

> [Leetcode 327. 区间和的个数](https://leetcode-cn.com/problems/count-of-range-sum/)

> 给定一个整数数组`nums`，返回区间和在`[lower,upper]`之间的个数，包含`lower`和`upper`。
>
> 区间和`S(i, j)`表示在`nums`中，位置从`i`到`j`的元素之和，包含`i`和`j`(`i≤j`)。
>
> 说明: 最直观的算法复杂度是$O(n^2)$，请在此基础上优化你的算法。
>
> 示例：
>
> ```
> 输入: nums = [-2,5,-1], lower = -2, upper = 2,
> 输出: 3 
> 解释: 3个区间分别是: [0,0], [2,2], [0,2]，它们表示的和分别为: -2, -1, 2。
> ```

**解析**：

==TODO==




# 二分搜索
## 二分查找
> [Leetcode 704. 二分查找](https://leetcode-cn.com/problems/binary-search/)

> 给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。

**解析**：在左边插入。

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        low, high = 0, len(nums)
        while low < high:
            mid = (low + high) // 2
            if nums[mid] < target:
                low = mid + 1
            else:
                high = mid
        
        if low >= len(nums) or nums[low] != target:
            return -1
        
        return low
```

## 搜索插入位置
> [Leetcode 35. 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)

> 给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

> 你可以假设数组中无重复元素。

**解析**：在左边插入。

```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        low, high = 0, len(nums)
        while low < high:
            mid = (low + high) // 2
            if nums[mid] < target:
                low = mid + 1
            else:
                high = mid
        
        return low
```

## 在排序数组中查找元素的第一个和最后一个位置
> [Leetcode 34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

> 给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。

> 你的算法时间复杂度必须是 O(log n) 级别。

> 如果数组中不存在目标值，返回 [-1, -1]。

**解析**：对于整个数组，使用二分搜索寻找**在左边插入**`target`的位置，该位置为**第一个位置**。
- 向右遍历，找到**最后一个位置**，时间复杂度为`O(n)`，不符合要求，因为题目要求时间复杂度必须是`O(logn)`级别。
- 对第一个位置右边的数组，使用二分搜索寻找**在右边插入**`target`的位置，该位置减一为**最后一个位置**，时间复杂度为`O(lgn)`。


使用两次二分搜索：
```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        if not nums:
            return [-1, -1]
        
        low, high = 0, len(nums)
        while low < high:
            mid = (low + high) // 2
            if nums[mid] < target:
                low = mid + 1
            else:
                high = mid
        
        if low >= len(nums) or nums[low] != target:
            return [-1, -1]

        low2, high2 = low + 1, len(nums)
        while low2 < high2:
            mid = (low2 + high2) // 2
            if target < nums[mid]:
                high2 = mid
            else:
                low2 = mid + 1

        return [low, high2 - 1]
```

## 寻找比目标字母大的最小字母
> [Leetcode 744. 寻找比目标字母大的最小字母](https://leetcode-cn.com/problems/find-smallest-letter-greater-than-target/)

> 给你一个排序后的字符列表 letters ，列表中只包含小写英文字母。另给出一个目标字母 target，请你寻找在这一有序列表里比目标字母大的最小字母。

> 在比较时，字母是依序循环出现的。举个例子：如果目标字母 target = 'z' 并且字符列表为 letters = ['a', 'b']，则答案返回 'a'

**解析**：在右边插入！

```
class Solution:
    def nextGreatestLetter(self, letters: List[str], target: str) -> str:
        low, high = 0, len(letters)
        while low < high:
            mid = (low + high) // 2
            if target < letters[mid]:
                high = mid
            else:
                low = mid + 1
        
        if low == len(letters):
            return letters[0]
            
        return letters[low]

```

## 山脉数组的峰顶索引
> [Leetcode 852. 山脉数组的峰顶索引](https://leetcode-cn.com/problems/peak-index-in-a-mountain-array/)

> 我们把符合下列属性的数组 A 称作山脉：

> - A.length >= 3
> - 存在 0 < i < A.length - 1 使得A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1]

> 给定一个确定为山脉的数组，返回任何满足 A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1] 的 i 的值。

**解析**：

```python
class Solution:
    def peakIndexInMountainArray(self, A: List[int]) -> int:
        low, high = 0, len(A)
        while low < high:
            mid = (low + high) // 2
            if A[mid-1] < A[mid] < A[mid+1]:
                low = mid
            elif A[mid-1] > A[mid] > A[mid+1]:
                high = mid
            else:
                return mid
```


## 寻找旋转排序数组中的最小值
> [Leetcode 153. 寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)

> 假设按照升序排序的数组在预先未知的某个点上进行了旋转。

> ( 例如，数组`[0,1,2,4,5,6,7]`可能变为`[4,5,6,7,0,1,2]`)。

> 请找出其中最小的元素。

> 你可以假设数组中不存在重复元素。

**解析**：

> 参考 [Leetcode官方题解](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/solution/xun-zhao-xuan-zhuan-pai-lie-shu-zu-zhong-de-zui-xi/)

```python
class Solution(object):
    def findMin(self, nums):
        if len(nums) == 1:
            return nums[0]

        left = 0
        right = len(nums) - 1
        if nums[left] < nums[right]:
            return nums[left]

        while left < right:
            mid = (left + right) // 2
            if nums[mid] > nums[mid + 1]:
                return nums[mid + 1]
            if nums[mid - 1] > nums[mid]:
                return nums[mid]
            if nums[mid] > nums[0]:
                left = mid + 1
            else:
                right = mid - 1
        return -1
```


## 搜索旋转排序数组
> [Leetcode 33. 搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

> 假设按照升序排序的数组在预先未知的某个点上进行了旋转。

> ( 例如，数组 `[0,1,2,4,5,6,7]` 可能变为 `[4,5,6,7,0,1,2]` )。

> 搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。

> 你可以假设数组中不存在重复的元素。

> 你的算法时间复杂度必须是 O(log n) 级别。

**解析**：分两种情况讨论。

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        if not nums:
            return -1
        
        left, right = 0, len(nums) - 1
        while left + 1 < right:
            mid = (left + right) // 2
            if nums[mid] == target:
                return mid
            elif nums[mid] > nums[left]:
                # case1: numbers between left and mid are sorted
                if nums[left] <= target < nums[mid]:
                    right = mid
                else:
                    left = mid + 1
            else:
                # case2: numbers between mid and right are sorted
                if nums[mid] < target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid
                    
        if nums[left] == target:
            return left
        elif nums[right] == target:
            return right
        return -1
```


## 有序数组中的单一元素
> [Leetcode 540. 有序数组中的单一元素](https://leetcode-cn.com/problems/single-element-in-a-sorted-array/)

> 给定一个只包含整数的有序数组，每个元素都会出现两次，唯有一个数只会出现一次，找出这个数。

> 示例 1:

```
输入: [1,1,2,3,3,4,4,8,8]
输出: 2
```

> 注意: 您的方案应该在 O(log n)时间复杂度和 O(1)空间复杂度中运行。

**解析**：计算元素个数。

```python
class Solution:
    def singleNonDuplicate(self, nums: List[int]) -> int:
        if len(nums) == 1:
            return nums[0]
        
        left, right = 0, len(nums) - 1
        while left < right:
            mid = (left + right) // 2
            if nums[mid] == nums[mid - 1]:
                if (mid - left) % 2 == 0:
                    right = mid - 2
                else:
                    left = mid + 1
            elif nums[mid] == nums[mid + 1]:
                if (right - mid) % 2 == 0:
                    left = mid + 2
                else:
                    right = mid - 1
            else:
                return nums[mid]
                
        return nums[left]
```


## x的平方根
> [Leetcode 69. x 的平方根](https://leetcode-cn.com/problems/sqrtx/)

> 实现 int sqrt(int x) 函数。

> 计算并返回 x 的平方根，其中 x 是非负整数。

> 由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。

**解析**：二分搜索完，可能比正常结果大1，再判断一下即可。

```python
class Solution:
    def mySqrt(self, x: int) -> int:
        low, high = 0, x
        while low < high:
            mid = (low + high) // 2
            if mid * mid < x:
                low = mid + 1
            else:
                high = mid
        
        if low ** 2 > x:
            return low - 1

        return low
```

给定精度限制：
```
class Solution:
    def mySqrt(self, x: int, threshold: float) -> int:
        low, high = 0, x
        while low + threshold < high:
            mid = (low + high) / 2
            if mid * mid < x:
                low = mid
            else:
                high = mid
        return low
```


## x的n次幂（快速幂）
> [Leetcode 50. Pow(x, n)](https://leetcode-cn.com/problems/powx-n/)

> 实现`pow(x, n)`，即计算 x 的 n 次幂函数。

**解析**：时间复杂度要求$O(\lg{n})$。

```python
class Solution:
    def myPow(self, x: float, n: int) -> float:        
        def quickMul(base, N):
            ans = 1.0
            while N > 0:
                if N & 1 == 1:
                    ans *= base
                base *= base
                N //= 2
            return ans
        
        return quickMul(x, n) if n >= 0 else 1.0 / quickMul(x, -n)
```


## 寻找两个正序数据的中位数
> 给定两个大小为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。

> 请你找出这两个正序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。

> 你可以假设 nums1 和 nums2 不会同时为空。

> 示例 1:
```
nums1 = [1, 3]
nums2 = [2]
则中位数是 2.0
```

**解析**：

> 参考 [Leetcode官方题解](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/solution/xun-zhao-liang-ge-you-xu-shu-zu-de-zhong-wei-s-114/)

如果不要求时间复杂度，要求找到两个有序数组的中位数，最直观的思路有以下两种：
- 把两个数组按序合并，寻找中位数，时间复杂度为O(m+n)，空间复杂度为O(m+n)。
- 不许要合并，用两个指针分别指向两个数组的下标为0的位置，每次比较并移动较小的指针，直到到达中位数的位置，此时时间复杂度为O(m+n)，空间复杂度为O(1)。

官方给出两种满足时间复杂度的方法，一是二分搜索，二是划分数组。

1、二分搜索
```
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        def getKthElement(k):         
            index1, index2 = 0, 0
            while True:
                # 特殊情况
                if index1 == m:
                    return nums2[index2 + k - 1]
                if index2 == n:
                    return nums1[index1 + k - 1]
                if k == 1:
                    return min(nums1[index1], nums2[index2])

                # 正常情况
                newIndex1 = min(index1 + k // 2 - 1, m - 1)
                newIndex2 = min(index2 + k // 2 - 1, n - 1)
                pivot1, pivot2 = nums1[newIndex1], nums2[newIndex2]
                if pivot1 <= pivot2:
                    k = k - (newIndex1 - index1 + 1)
                    index1 = newIndex1 + 1
                else:
                    k = k - (newIndex2 - index2 + 1)
                    index2 = newIndex2 + 1
        
        m, n = len(nums1), len(nums2)
        totalLength = m + n
        if totalLength % 2 == 1:
            return getKthElement((totalLength + 1) // 2)
        return (getKthElement(totalLength // 2) + getKthElement(totalLength // 2 + 1)) / 2
```




# 动态规划
## 最长公共子序列
> [Leetcode 1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)

> 给定两个字符串 text1 和 text2，返回这两个字符串的最长公共子序列的长度。

> 一个字符串的子序列是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。

> 例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。两个字符串的「公共子序列」是这两个字符串所共同拥有的子序列。

> 若这两个字符串没有公共子序列，则返回 0。

**解析**：

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        n1 = len(text1)
        n2 = len(text2)
        dp = [[0] * (n2 + 1) for _ in range(n1 + 1)]
        for i in range(n1):
            for j in range(n2):
                if text1[i] == text2[j]:
                    dp[i + 1][j + 1] = dp[i][j] + 1
                else:
                    dp[i + 1][j + 1] = max(dp[i + 1][j], dp[i][j + 1])

        return dp[-1][-1]
```

输出最长公共子序列：
```python
def print_lcs(text1, text2, dp):
    '''dp为求text1和text2最长公共子序列的状态矩阵'''
    n1 = len(text1)
    n2 = len(text2)
    lcs = []
    i = n1
    j = n2
    while i != 0 and j != 0:
        while dp[i - 1][j] == dp[i][j]:  # 循环1，可以和循环2交换
            i -= 1
        while dp[i][j - 1] == dp[i][j]:  # 循环2
            j -= 1
        lcs.append(text2[j - 1])
        i -= 1
        j -= 1
    return "".join(lcs[::-1])
```


## 最长回文子串
> [Leetcode 5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

> 给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

**解析**：用`dp[i][j]`表示`s`中第`i`个字符到第`j`个字符组成的子串是否是回文子串。

1、动态规划
```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        longest_len = 0
        longest_pali = ""
        n = len(s)
        dp = [[0 for _ in range(n)] for _ in range(n)]
        for length in range(1, n + 1):
            for i in range(0, n - length + 1):
                j = i + length - 1
                if length <= 2:
                    dp[i][j] = s[i] == s[j]
                else:
                    dp[i][j] = dp[i + 1][j - 1] & (s[i] == s[j])
                if dp[i][j] and length > longest_len:
                    longest_len = length
                    longest_pali = s[i:i + length]
        return longest_pali
```

2、中心扩展
```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        def findP(string, left, right):
            while left >= 0 and right < len(string) and string[left] == string[right]:
                left -= 1
                right += 1
            return right - left -1

        if not s:
            return ""
        
        start = end = 0
        res = 0
        for i in range(len(s)-1):
            len1 = findP(s, i, i)
            len2 = findP(s, i, i + 1)
            length = max(len1, len2)
            if length > res:
                res = length
                start = i - (length - 1) // 2
                end = i + length // 2

        return s[start:end + 1]
```


## 最长回文子序列
> [Leetcode 516. 最长回文子序列](https://leetcode-cn.com/problems/longest-palindromic-subsequence/)

> 给定一个字符串 s，找到其中最长的回文子序列，并返回该序列的长度。可以假设 s 的最大长度为 1000 。

**解析**：用`dp[i][j]`表示`s`中第`i`个字符到第`j`个字符组成的子串的最长的回文序列的长度。

状态转移方程为：
- 当`i=j`，即当前子串的长度为1，`dp[i][j]=1`；
- 当`i<j`，即当前子串的长度大于等于2：
    - 如果`s[i]=s[j]`，则`dp[i][j]=dp[i+1][j-1]+2`；
    - 如果`s[i]!=s[j]`，则`dp[i][j]=max(dp[i+1][j],dp[i][j-1])`；

1、使用最长回文子串的思路

```python
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        n = len(s)
        dp = [[0 for _ in range(n)] for _ in range(n)]
        for length in range(1, n + 1):
            for i in range(0, n - length + 1):
                if length == 1:
                    dp[i][i] = 1
                    continue
                j = i + length - 1
                if s[i] == s[j]:
                    dp[i][j] = dp[i + 1][j - 1] + 2
                else:
                    dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])

        return dp[0][n - 1]
```

2、从后向前遍历

> 参考 [Leetcode-元仲辛](https://leetcode-cn.com/problems/longest-palindromic-subsequence/solution/dong-tai-gui-hua-si-yao-su-by-a380922457-3/)

```python
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        n = len(s)
        dp = [[0 for _ in range(n)] for _ in range(n)]
        for i in range(n - 1, -1, -1):
            dp[i][i] = 1
            for j in range(i + 1, n):
                if s[i] == s[j]:
                    dp[i][j] = dp[i + 1][j - 1] + 2
                else:
                    dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])
        return dp[0][n - 1]
```


## 最长上升子序列
> [Leetcode 300. 最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

> 给定一个无序的整数数组，找到其中最长上升子序列的长度。

**解析**：

> 参考 [Leetcode-Solution](https://leetcode-cn.com/problems/longest-increasing-subsequence/solution/zui-chang-shang-sheng-zi-xu-lie-by-leetcode-soluti/)

1、动态规划

定义`dp[i]`为考虑数组前`i`个数字，以第`i`个数字结尾的最长上升子序列的长度，注意`nums[i]`必须被选取。

从小到大计算`dp`数组的值，在计算`dp[i]`之前，我们已经计算出`dp[0...i-1]`的值，则状态转移方程为：
```
dp[i] = max(dp[j]) + 1 0 <= j < i & num[j] < num[i]
```

最后，整个数组的最长上升子序列即所有`dp[i]`中的最大值。

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        if not nums:
            return 0
        dp = [1] * len(nums)
        for i in range(1, len(nums)):
            for j in range(i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[i], dp[j] + 1)
                    
        return max(dp)
```

2、贪心 + 二分查找。如果我们要使上升子序列尽可能的长，则我们需要让序列上升得尽可能慢，因此我们希望每次在上升子序列最后加上的那个数尽可能的小。

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        seen = []
        for num in nums:
            if not seen or num > seen[-1]:
                seen.append(num)
            else:
                left, right = 0, len(seen)
                while left < right:
                    mid = (left + right) // 2
                    if seen[mid] < num:
                        left = mid + 1
                    else:
                        right = mid
                seen[left] = num
        return len(seen)
```


## 最长连续序列
> [Leetcode 128. 最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)

> 给定一个未排序的整数数组，找出最长连续序列的长度。

> 要求算法的时间复杂度为 O(n)。

**解析**：哈希表！

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


## 零钱兑换
### 零钱兑换
> [Leetcode 322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)

> 给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回-1。

**解析**：

```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [float('inf')] * (amount + 1)
        dp[0] = 0
        
        for coin in coins:
            for x in range(coin, amount + 1):
                dp[x] = min(dp[x], dp[x - coin] + 1)
                
        return dp[amount] if dp[amount] != float('inf') else -1
```


### 零钱兑换 II
> [Leetcode 518. 零钱兑换 II](https://leetcode-cn.com/problems/coin-change-2/)

> 给定不同面额的硬币和一个总金额。写出函数来计算可以凑成总金额的硬币组合数。假设每一种面额的硬币有无限个。

**解析**：

```python
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        dp = [0] * (amount + 1)
        dp[0] = 1
        
        for coin in coins:
            for x in range(coin, amount + 1):
                dp[x] += dp[x - coin]
        return dp[amount]
```


## 打家劫舍
### 打家劫舍
> [Leetcode 198. 打家劫舍](https://leetcode-cn.com/problems/house-robber/)

> 你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警**。

> 给定一个代表每个房屋存放金额的非负整数数组，计算你**在不触动警报装置的情况下**，一夜之内能够偷窃到的最高金额。

**解析**：用`dp[i]`表示前`i`间房屋能偷窃到的最高总金额，状态转移方程为`dp[i]=max(dp[i−2]+nums[i],dp[i−1])`。

1、空间复杂度O(n)
```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        if not nums:
            return 0
        elif len(nums) <= 2:
            return max(nums)
            
        dp = [0] * len(nums)
        dp[0] = nums[0]
        dp[1] = max(nums[0], nums[1])
        for i in range(2, len(nums)):
            dp[i] = max(dp[i - 2] + nums[i], dp[i - 1])

        return dp[-1]
```

2、空间复杂度O(1)
```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        if not nums:
            return 0
        elif len(nums) <= 2:
            return max(nums)
            
        pre = nums[0]
        cur = max(nums[0], nums[1])
        for n in nums[2:]:
            pre, cur = cur, max(n + pre, cur)
            
        return cur
```


### 打家劫舍II
> [Leetcode 213. 打家劫舍 II](https://leetcode-cn.com/problems/house-robber-ii/)

> 你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都**围成一圈**，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警**。

> 给定一个代表每个房屋存放金额的非负整数数组，计算你**在不触动警报装置的情况下**，能够偷窃到的最高金额。

**解析**：头尾两个房屋不能同时抢。要么不抢第一个，`nums[1:]`；要么不抢最后一个，`nums[:-1]`。

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        def helper(nums):
            dp = [0] * len(nums)
            dp[0] = nums[0]
            dp[1] = max(nums[0], nums[1])
            for i in range(2, len(nums)):
                dp[i] = max(dp[i - 2] + nums[i], dp[i - 1])
            return dp[-1]

        if not nums:
            return 0
        elif len(nums) <= 2:
            return max(nums)

        return max(helper(nums[1:]), helper(nums[:-1]))
```


### 打家劫舍III
> [Leetcode 337. 打家劫舍 III](https://leetcode-cn.com/problems/house-robber-iii/)

> 在上次打劫完一条街道之后和一圈房屋后，小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为“根”。 除了“根”之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果两个直接相连的房子在同一天晚上被打劫，房屋将自动报警。

> 计算在不触动警报的情况下，小偷一晚能够盗取的最高金额。

**解析**：

> 参考 [Leetcode-reals](https://leetcode-cn.com/problems/house-robber-iii/solution/san-chong-fang-fa-jie-jue-shu-xing-dong-tai-gui-hu/)

1、递归，超时。
```python
class Solution:
    def rob(self, root: TreeNode) -> int:
        def helper(node):
            if not node:
                return 0

            money = node.val
            if node.left is not None:
                money += helper(node.left.left) + helper(node.left.right)

            if node.right is not None:
                money += helper(node.right.left) + helper(node.right.right)

            return max(money, helper(node.left) + helper(node.right))

        return helper(root)
```

2、带记忆的递归，通过。
```python
class Solution:
    def rob(self, root: TreeNode) -> int:
        def helper(node, memo):
            if not node:
                return 0
            if node in memo:
                return memo[node]

            money = node.val
            if node.left is not None:
                money += helper(node.left.left, memo) + helper(node.left.right, memo)
            if node.right is not None:
                money += helper(node.right.left, memo) + helper(node.right.right, memo)

            res = max(money, helper(node.left, memo) + helper(node.right, memo))
            memo[node] = res
            return res

        memo = {}
        return helper(root, memo)
```

3、终极解法，使用了二叉树的一种递归模板。
```python
class Solution:
    def rob(self, root: TreeNode) -> int:
        def helper(node):
            if not node:
                return 0, 0  # max money if not rob cur node, max money if rob cur node
            
            left = helper(node.left)
            right = helper(node.right)
            not_rob_cur_node = max(left[0], left[1]) + max(right[0], right[1])
            rob_cur_node = left[0] + right[0] + node.val
            return not_rob_cur_node, rob_cur_node

        res = helper(root)
        return max(res)
```


## 买卖股票
### 买卖股票
> [Leetcode 121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

> 给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

> 如果你最多只允许完成一笔交易（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。

> 注意：你不能在买入股票前卖出股票。

**解析**：记录【今天之前买入的最小值】，【最大获利】。计算【今天之前最小值买入，今天价格卖出的获利】，和【最大获利】进行比较。比较【今天价格】和【今天之前买入的最小值】，如果前者小于后者，则更新后者。

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if not prices or len(prices) <= 1:
            return 0
        
        min_price = prices[0]
        max_profit = 0
        for price in prices[1:]:
            if price < min_price:
                min_price = price
            max_profit = max(max_profit, price - min_price)
        
        return max_profit
```


### 买卖股票II
> [Leetcode 122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

> 给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。
>
> 设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。
>
> 注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

**解析**：可以同一天买入和卖出，先卖出，再买入。

1、直接方法
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if not prices or len(prices) <= 1:
            return 0
        
        min_price = prices[0]
        max_profit = 0
        for price in prices[1:]:
            if min_price < price:
                max_profit += price - min_price
            min_price = price

        return max_profit
```

2、比较两个相邻的元素
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        max_profit = 0
        for i in range(1, len(prices)):
            if prices[i] > prices[i - 1]:
                max_profit += prices[i] - prices[i - 1]
        return max_profit
```


## 爬楼梯
### 爬楼梯
> [Leetcode 70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

> 假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

> 每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

> 注意：给定 n 是一个正整数。

**解析**：状态转移方程为`dp[i] = dp[i - 1] + dp[i - 2], i >= 2`。

1、空间复杂度O(n)
```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n <= 1:
            return n
        
        dp = [0] * n
        dp[0] = 1
        dp[1] = 2
        for i in range(2, n):
            dp[i] = dp[i - 1] + dp[i - 2]
        return dp[n - 1]
```

2、空间复杂度O(1)
```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n <= 1:
            return n
        
        pre, cur = 1, 2
        for i in range(2, n):
            pre, cur = cur, pre + cur
        return cur
```

### 使用最小花费爬楼梯
> [Leetcode 746. 使用最小花费爬楼梯](https://leetcode-cn.com/problems/min-cost-climbing-stairs/)

> 数组的每个索引作为一个阶梯，第i个阶梯对应着一个非负数的体力花费值cost[i]（索引从0开始）。

> 每当你爬上一个阶梯你都要花费对应的体力花费值，然后你可以选择继续爬一个阶梯或者爬两个阶梯。

> 您需要找到达到楼层顶部的最低花费。在开始时，你可以选择从索引为 0 或 1 的元素作为初始阶梯。

**解析**：
- 要走到顶楼，所有台阶都可能走。
- 转移方程为：`dp[i] = min(dp[i-1], dp[i-2]) + cost[i], i>=2`。
- 最后选择最后一个台阶和倒数第二个台阶的较小值。 

1、空间复杂度O(n)
```python
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        n = len(cost)
        dp = [0] * n
        dp[0] = cost[0]
        dp[1] = cost[1]
        for i in range(2, n):
            dp[i] = min(dp[i - 1], dp[i - 2]) + cost[i]
        return min(dp[n - 2], dp[n - 1])
```

2、空间复杂度O(1)
```python
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        pre, cur = cost[0], cost[1]
        for c in cost[2:]:
            pre, cur = cur, min(pre, cur) + c
        return min(pre, cur)
```


## 背包问题
> [背包九讲——全篇详细理解与代码实现](https://blog.csdn.net/yandaoqiusheng/article/details/84782655/)

问题描述：给定一个容量为`capacity`的背包和`N`种物品，物品的重量（体积）用一个长度为`N`的数组`weights`表示，物品的价值用一个长度为`N`的数组`values`表示，第`i`种物品的重量和价值分别为`weights[i]`和`values[i]`。请问，在不超过背包容量的条件下，如何选择物品，使得价值最大。

代码格式非Leetcode！

### 0/1背包 I
> 不要求背包恰好装满。每种物品最多选择1个，意思可以不选择或者选择一个。

1、使用二维数组
```python
def zero_one_backpack(weights, values, capacity):
    n = len(weights)
    dp = [[0 for _ in range(capacity + 1)] for _ in range(n + 1)]
    for i in range(n):  # 对于每一个物品
        for j in range(1, capacity + 1):  # 对于每一个容量
            if j < weights[i]:
                dp[i + 1][j] = dp[i][j]
            else:
                dp[i + 1][j] = max(dp[i][j], dp[i][j - weights[i]] + values[i])
    return dp[-1][-1]
```

2、使用一维数组
```python
def zero_one_backpack(weights, values, capacity):
    n = len(weights)
    dp = [0 for _ in range(capacity + 1)]
    for i in range(n):
        for j in range(capacity, weights[i] - 1, -1):
            if i == 0:
                dp[j] = values[i]
            else:
                dp[j] = max(dp[j], dp[j - weights[i]] + values[i])
    return dp[-1]
```


### 0/1背包 II
> 要求背包恰好装满。每种物品最多选择1个，意思可以不选择或者选择一个。

1、使用二维数组
```python
def zero_one_backpack_full(weights, values, capacity):
    n = len(weights)
    dp = [[0] + [-1 for _ in range(capacity)] for _ in range(n + 1)]
    for i in range(n):
        for j in range(1, capacity + 1):
            if j < weights[i]:
                dp[i + 1][j] = dp[i][j]
            elif dp[i][j - weights[i]] != -1:
                dp[i + 1][j] = max(dp[i][j], dp[i][j - weights[i]] + values[i])
    return dp[-1][-1]
```

2、使用一维数组
```python
def zero_one_backpack_full(weights, values, capacity):
    n = len(weights)
    dp = [-1 for _ in range(capacity + 1)]
    dp[0] = 0
    for i in range(n):
        for j in range(capacity, weights[i] - 1, -1):
            if dp[i][j - weights[i]] != -1:
                dp[j] = max(dp[j], dp[j - weights[i]] + values[i])
    return dp[-1]
```


### 完全背包
> 每种物品可以选择多个，在不超过背包容量的条件下。

1、使用一维数组
```python
def complete_backpack(weights, values, capacity):
    n = len(weights)
    dp = [0] * (capacity + 1)
    for i in range(n):
        for j in range(capacity + 1):
            for k in range(j // weights[i] + 1):
                dp[j] = max(dp[j], dp[j - k * weights[i]] + k * values[i])
    return dp[-1]
```

2、使用一维数组
```python
def complete_backpack_2(weights, values, capacity):
    n = len(weights)
    dp = [0] * (capacity + 1)
    for i in range(n):
        for j in range(weights[i], capacity + 1):
            dp[j] = max(dp[j], dp[j - weights[i]] + values[i])
    return dp[-1]
```


## 解码方法
> [Leetcode 91. 解码方法](https://leetcode-cn.com/problems/decode-ways/)

> 一条包含字母 A-Z 的消息通过以下方式进行了编码：
```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```

> 给定一个只包含数字的非空字符串，请计算解码方法的总数。

> 示例 1:
```
输入: "12"
输出: 2
解释: 它可以解码为 "AB"（1 2）或者 "L"（12）。
```

**解析**：用`dp[i]`表示字符串第`0`个字符到第`i`个字符组成的子串的解码的总数，`dp[0]`和`dp[1]`可以直接求出。状态转移方程参考代码。

```python
class Solution:
    def numDecodings(self, s: str) -> int:
        if not s or s[0] == "0":
            return 0
        if len(s) == 1:
            return 1
        
        n = len(s)
        dp = [0] * n
        dp[0] = 1
        num = int(s[0:2])
        if num == 10 or num == 20:
            dp[1] = 1
        elif 11 <= num <= 19 or 21 <= num <= 26:
            dp[1] = 2
        elif s[1] == "0" :
            dp[1] = 0
        else:
            dp[1] = 1

        for i in range(2, n):
            if s[i] != "0":
                dp[i] = dp[i - 1]
            num = int(s[i - 1:i + 1])
            if 10 <= num <= 26:
                dp[i] += dp[i - 2]

        return dp[n - 1]
```


## 把数字翻译成字符串
> [Leetcode 剑指 Offer 46. 把数字翻译成字符串](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)

> 给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

> 示例：

```
输入: 12258
输出: 5
解释: 12258有5种不同的翻译，分别是"bccfi", "bwfi", "bczi", "mcfi"和"mzi"
```

**解析**：用`dp[i]`表示表示字符串第`0`个字符到第`i`个字符组成的子串的翻译的总数，`dp[0]`和`dp[1]`可以直接求出。状态转移方程参考代码。

```python
class Solution:
    def translateNum(self, num: int) -> int:
        if 0 <= num < 10:
            return 1
        str_num = str(num)
        n = len(str_num)
        dp = [1] * n
        if 10 <= int(str_num[:2]) <= 25:
            dp[1] = 2
        for i in range(2, n):
            if 10 <= int(str_num[i - 1:i + 1]) <= 25:
                dp[i] = dp[i - 2] + dp[i - 1]
            else:
                dp[i] = dp[i - 1]
        return dp[n - 1]
```


## 不同的二叉搜索树
> [Leetcode 96. 不同的二叉搜索树](https://leetcode-cn.com/problems/unique-binary-search-trees/)

> 给定一个整数 n，求以 1 ... n 为节点组成的二叉搜索树有多少种？

**解析**：

```python
class Solution:
    def numTrees(self, n: int) -> int:
        dp = [0] * (n + 1)
        dp[0] = 1
        dp[1] = 1
        for i in range(2, n + 1):
            for j in range(1, i + 1):
                dp[i] += dp[j - 1] * dp[i - j]
        return dp[n]
```


## 矩阵中的最长递增路径
> [Leetcode 329. 矩阵中的最长递增路径](https://leetcode-cn.com/problems/longest-increasing-path-in-a-matrix/)

> 给定一个整数矩阵，找出最长递增路径的长度。

> 对于每个单元格，你可以往上，下，左，右四个方向移动。你不能在对角线方向上移动或移动到边界外（即不允许环绕）。

**解析**：动态规划，从小到大进行访问。

> 参考 [Leetcode-gyx2110](https://leetcode-cn.com/problems/longest-increasing-path-in-a-matrix/comments/96976)

```python
class Solution:
    def longestIncreasingPath(self, matrix: List[List[int]]) -> int:
        if not matrix or not matrix[0]:
            return 0
        m, n = len(matrix), len(matrix[0])
        nums = [[matrix[i][j], i, j] for i in range(m) for j in range(n)]
        nums.sort(key=lambda x: x[0])
        
        dp = [[1 for _ in range(n)] for _ in range(m)]
        for num, i, j in nums:
            for dx, dy in ((0, 1), (0, -1), (1, 0), (-1, 0)):
                neigh_x = i + dx
                neigh_y = j + dy
                if 0 <= neigh_x < m and 0 <= neigh_y < n:
                    if matrix[i][j] > matrix[neigh_x][neigh_y]:
                        dp[i][j] = max(dp[i][j], dp[neigh_x][neigh_y] + 1)
        return max(dp[i][j] for i in range(m) for j in range(n))
```


## 矩阵中最长的连续1线段
> [LeetCode 562. 矩阵中最长的连续1线段](https://leetcode-cn.com/problems/longest-line-of-consecutive-one-in-matrix/)

> 给定一个01矩阵 M，找到矩阵中最长的连续1线段。这条线段可以是水平的、垂直的、对角线的或者反对角线的。

**注意**：Leetcode需要会员。

**解析**：

> 参考 [LeetCode 562. 矩阵中最长的连续1线段（DP）](https://blog.csdn.net/qq_21201267/article/details/107205044)

```python
class Solution:
    def longestLine(M):
        if not M or not M[0]:
            return 0
        maxlen = 0
        m = len(M)
        n = len(M[0])
        dp_hori = [[0 for _ in range(n)] for _ in range(m)]
        dp_veti = [[0 for _ in range(n)] for _ in range(m)]
        dp_diag = [[0 for _ in range(n)] for _ in range(m)]
        dp_anti_diag = [[0 for _ in range(n)] for _ in range(m)]
        for i in range(m):
            for j in range(n):
                if M[i][j] == 1:
                    dp_hori[i][j] = dp_hori[i - 1][j] + 1 if i - 1 >= 0 else 1
        			dp_veti[i][j] = dp_veti[i][j - 1] + 1 if j - 1 >= 0 else 1
        			dp_diag[i][j] = dp_diag[i - 1][j + 1] + 1  if i - 1 >= 0 and j + 1 < n else 1
        			dp_anti_diag[i][j] = dp_anti_diag[i - 1][j - 1] + 1 if i - 1 >= 0 and j - 1 >= 0 else 1
        			maxlen = max(maxlen, dp_hori[i][j], dp_veti[i][j], 
        			             dp_diag[i][j], dp_anti_diag[i][j])
    	return maxlen
```


## 最大正方形
> [Leetcode 221. 最大正方形](https://leetcode-cn.com/problems/maximal-square/)

> 在一个由 0 和 1 组成的二维矩阵内，找到只包含 1 的最大正方形，并返回其面积。

> 示例：

```
输入: 
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
输出: 4
```

**解析**：

```python
class Solution:
    def maximalSquare(self, matrix: List[List[str]]) -> int:
        if len(matrix) == 0 or len(matrix[0]) == 0:
            return 0
        
        max_side = 0
        m, n = len(matrix), len(matrix[0])
        dp = [[0] * n for _ in range(m)]
        for i in range(m):
            for j in range(n):
                if matrix[i][j] == '1':
                    if i == 0 or j == 0:
                        dp[i][j] = 1
                    else:
                        dp[i][j] = min(dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1]) + 1
                    max_side = max(max_side, dp[i][j])
        return max_side * max_side
```


## 统计全为 1 的正方形子矩阵
> [Leetcode 1277. 统计全为 1 的正方形子矩阵](https://leetcode-cn.com/problems/count-square-submatrices-with-all-ones/)

> 给你一个 m * n 的矩阵，矩阵中的元素不是 0 就是 1，请你统计并返回其中完全由 1 组成的 正方形 子矩阵的个数。

**解析**：

> 参考 [Leetcode官方题解](https://leetcode-cn.com/problems/count-square-submatrices-with-all-ones/solution/tong-ji-quan-wei-1-de-zheng-fang-xing-zi-ju-zhen-2/)

```python
class Solution:
    def countSquares(self, matrix: List[List[int]]) -> int:
        m, n = len(matrix), len(matrix[0])
        dp = [[0] * n for _ in range(m)]
        res = 0
        for i in range(m):
            for j in range(n):
                if matrix[i][j] == 1:
                    if i == 0 or j == 0:
                        dp[i][j] = matrix[i][j]
                    else:
                        dp[i][j] = min(dp[i][j - 1], dp[i - 1][j], dp[i - 1][j - 1]) + 1
                    res += dp[i][j]
        return res
```


## 最大矩形
> [Leetcode 85. 最大矩形](https://leetcode-cn.com/problems/maximal-rectangle/)

> 给定一个仅包含 0 和 1 的二维二进制矩阵，找出只包含 1 的最大矩形，并返回其面积。

> 示例:

```
输入:
[
  ["1","0","1","0","0"],
  ["1","0","1","1","1"],
  ["1","1","1","1","1"],
  ["1","0","0","1","0"]
]
输出: 6
```

**解析**：

```python
==TODO==
```


## 等差数列划分
> [Leetcode 413. 等差数列划分](https://leetcode-cn.com/problems/arithmetic-slices/)

> 如果一个数列至少有三个元素，并且任意两个相邻元素之差相同，则称该数列为等差数列。

> 函数要返回数组 A 中所有为等差数组的子数组个数。

> 示例:

```
A = [1, 2, 3, 4]
返回: 3, A 中有三个子等差数组: [1, 2, 3], [2, 3, 4] 以及自身 [1, 2, 3, 4]。
```

**解析**：

```python
class Solution:
    def numberOfArithmeticSlices(self, A: List[int]) -> int:
        n = len(A)
        dp = [0] * n
        res = 0
        for i in range(2, n):
            if A[i] - A[i - 1] == A[i - 1] - A[i - 2]:
                dp[i] = dp[i - 1] + 1
                res += dp[i]
            else:
                dp[i] = 0
        return res
```


## 等差数列划分 II - 子序列
> [Leetcode 446. 等差数列划分 II - 子序列](https://leetcode-cn.com/problems/arithmetic-slices-ii-subsequence/)

> 如果一个数列至少有三个元素，并且任意两个相邻元素之差相同，则称该数列为等差数列。

> 函数要返回数组 A 中所有等差子序列的个数。

> 输入包含 N 个整数。每个整数都在 -231 和 231-1 之间，另外 0 ≤ N ≤ 1000。保证输出小于 231-1。

> 示例：
```
输入：[2, 4, 6, 8, 10]
输出：7

解释：
所有的等差子序列为：
[2,4,6]
[4,6,8]
[6,8,10]
[2,4,6,8]
[4,6,8,10]
[2,4,6,8,10]
[2,6,10]
```

**解析**：

> 参考 [Leetcode官方题解](https://leetcode-cn.com/problems/arithmetic-slices-ii-subsequence/solution/deng-chai-shu-lie-hua-fen-ii-zi-xu-lie-by-leetcode/)

```python
class Solution:
    def numberOfArithmeticSlices(self, A: List[int]) -> int:
        n = len(A)
        res = 0
        counter = [{} for _ in range(n)]
        for i in range(n):
            for j in range(i):
                delta = A[i] - A[j]
                pre = counter[j].get(delta, 0)
                cur = counter[i].get(delta, 0)
                counter[i][delta] = pre + cur + 1
                res += pre
        return res
```


## 最长定差子序列
> [Leetcode 1218. 最长定差子序列](https://leetcode-cn.com/problems/longest-arithmetic-subsequence-of-given-difference/)

> 给你一个整数数组`arr`和一个整数`difference`，请你找出`arr`中所有相邻元素之间的差等于给定`difference`的等差子序列，并返回其中最长的等差子序列的长度。

> 示例 1：
```
输入：arr = [1,2,3,4], difference = 1
输出：4
解释：最长的等差子序列是 [1,2,3,4]。
```

**解析**：

> 参考 [Leetcode-太阳家的猫](https://leetcode-cn.com/problems/longest-arithmetic-subsequence-of-given-difference/comments/158881)

```
class Solution:
    def longestSubsequence(self, arr: List[int], difference: int) -> int:
        dp = {}
        res = 0
        for a in arr:
            dp[a] = dp.get(a - difference, 0) + 1
            res = max(res, dp[a])
        return res
```


## 不同的子序列
> [Leetcode 115. 不同的子序列](https://leetcode-cn.com/problems/distinct-subsequences/)

> 给定一个字符串 S 和一个字符串 T，计算在 S 的子序列中 T 出现的个数。

> 一个字符串的一个子序列是指，通过删除一些（也可以不删除）字符且不干扰剩余字符相对位置所组成的新字符串。（例如，"ACE" 是 "ABCDE" 的一个子序列，而 "AEC" 不是）

> 题目数据保证答案符合 32 位带符号整数范围。

> 示例 1：
```
输入：S = "rabbbit", T = "rabbit"
输出：3
解释：

如下图所示, 有 3 种可以从 S 中得到 "rabbit" 的方案。
(上箭头符号 ^ 表示选取的字母)

rabbbit
^^^^ ^^
rabbbit
^^ ^^^^
rabbbit
^^^ ^^^
```

**解析**：

> 参考 [Leetcode-powcai](https://leetcode-cn.com/problems/distinct-subsequences/solution/dong-tai-gui-hua-by-powcai-5/)

> 参考 [Leetcode-云想衣裳花想容](https://leetcode-cn.com/problems/distinct-subsequences/solution/dong-tai-gui-hua-shou-dong-mo-ni-dong-tai-zhuan-yi/)

用`dp[i][j]`表示对于字符串T中第`0`个字符到第`i`字符组成的子串出现在字符串S中第`0`个字符到第`j`字符组成的子串的子序列的次数。

转移方程为：
```
dp[i][j] = dp[i-1][j-1] + dp[i][j-1] if S[j] == T[i];
dp[i][j] = dp[i][j-1], if S[j] != T[i];
```

对于dp矩阵的第一行，T 为空，因为空集是所有字符串子集，所以第一行都是1。对于dp矩阵的第一列, S 为空，所以第一列都是0。

```python
class Solution:
    def numDistinct(self, s: str, t: str) -> int:
        n1 = len(s)
        n2 = len(t)
        dp = [[0] * (n1 + 1) for _ in range(n2 + 1)]
        for j in range(n1 + 1):
            dp[0][j] = 1
        for i in range(1, n2 + 1):
            for j in range(1, n1 + 1):
                if t[i - 1] == s[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1]  + dp[i][j - 1]
                else:
                    dp[i][j] = dp[i][j - 1]
        return dp[-1][-1]
```


## 计算各个位数不同的数字个数
> [Leetcode 357. 计算各个位数不同的数字个数](https://leetcode-cn.com/problems/count-numbers-with-unique-digits/)

> 给定一个非负整数 n，计算各位数字都不同的数字 x 的个数，其中 0 ≤ x < 10n 。

> 示例:
```
输入: 2
输出: 91 
解释: 答案应为除去 11,22,33,44,55,66,77,88,99 外，在 [0,100) 区间内的所有数字。
```

**解析**：

```
class Solution:
    def countNumbersWithUniqueDigits(self, n: int) -> int:
        if n == 0:
            return 1
        dp = [0] * (n + 1)
        dp[0] = 1
        dp[1] = 10
        for i in range(2, n + 1):
            dp[i] = (dp[i - 1] - dp[i - 2]) * (10 - (i - 1)) + dp[i-1]
        return dp[n]
```


# 贪心
## 跳跃游戏
### 跳跃游戏
> [Leetcode 55. 跳跃游戏](https://leetcode-cn.com/problems/jump-game/)

> 给定一个非负整数数组，你最初位于数组的第一个位置。

> 数组中的每个元素代表你在该位置可以跳跃的最大长度。

> 判断你是否能够到达最后一个位置。

**解析**：

> 参考 [Leetcode官方题解](https://leetcode-cn.com/problems/jump-game/solution/tiao-yue-you-xi-by-leetcode-solution/)

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        n = len(nums)
        rightmost = 0
        for i in range(n):
            if i <= rightmost:
                rightmost = max(rightmost, i + nums[i])
                if rightmost >= n - 1:
                    return True
        return False
```


### 跳跃游戏 II
> [Leetcode 45. 跳跃游戏 II](https://leetcode-cn.com/problems/jump-game-ii/)

> 给定一个非负整数数组，你最初位于数组的第一个位置。

> 数组中的每个元素代表你在该位置可以跳跃的最大长度。

> 你的目标是使用最少的跳跃次数到达数组的最后一个位置。

> 说明: 假设你总是可以到达数组的最后一个位置。

**解析**：

> 参考 [Leetcode官方题解](https://leetcode-cn.com/problems/jump-game-ii/solution/tiao-yue-you-xi-ii-by-leetcode-solution/)

1、方法一，反向查找出发位置，时间复杂度$O(n^2)$。用Python编写会超时，所以给出了Java实现。
```java
class Solution {
    public int jump(int[] nums) {
        int position = nums.length - 1;
        int steps = 0;
        while (position > 0) {
            for (int i = 0; i < position; i++) {
                if (i + nums[i] >= position) {
                    position = i;
                    steps++;
                    break;
                }
            }
        }
        return steps;
    }
}
```

2、方法二，正向查找可到达的最大位置，时间复杂度$O(n)$。
```python
class Solution:
    def jump(self, nums: List[int]) -> int:
        n = len(nums)
        max_pos = 0
        cur_end = 0
        steps = 0
        for i in range(n - 1):
            if max_pos >= i:
                max_pos = max(max_pos, i + nums[i])
                if i == cur_end:
                    cur_end = max_pos
                    steps += 1
        return steps
```


## 坏了的计算器
> [Leetcode 991. 坏了的计算器](https://leetcode-cn.com/problems/broken-calculator/)

> 在显示着数字的坏计算器上，我们可以执行以下两种操作：
> - 双倍（Double）：将显示屏上的数字乘 2；
> - 递减（Decrement）：将显示屏上的数字减 1 。

> 最初，计算器显示数字 X。
>
> 返回显示数字 Y 所需的最小操作数。

**解析**：逆向思维。从大的数字到小的数字比较简单，偶数就除以2，奇数就加1。

```python
class Solution:
    def brokenCalc(self, X: 'int', Y: 'int') -> 'int':
        res = 0
        while Y > X:
            res += 1
            if Y % 2 == 1:
                Y += 1
            else:
                Y //= 2

        return res + X - Y
```


# DFS
## 岛屿数量
> [Leetcode 200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

> 给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。

> 岛屿总是被水包围，并且每座岛屿只能由水平方向或竖直方向上相邻的陆地连接形成。

> 此外，你可以假设该网格的四条边均被水包围。

**解析**：

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        def dfs(grid, r, c):
            if not (0 <= r < len(grid) and 0 <= c < len(grid[0])):
                return 
            elif grid[r][c] == '0':
                return 
            elif grid[r][c] != '1':  # grid[r][c] == 2
                return 
            grid[r][c] = '2'
            dfs(grid, r-1, c)
            dfs(grid, r+1, c)
            dfs(grid, r, c-1)
            dfs(grid, r, c+1)

        count = 0
        for row in range(len(grid)):
            for col in range(len(grid[0])):
                if grid[row][col] == '1':
                    count += 1
                    dfs(grid, row, col)

        return count
```


## 岛屿的最大面积
> [Leetcode 695. 岛屿的最大面积](https://leetcode-cn.com/problems/max-area-of-island/)

> 给定一个包含了一些 0 和 1 的非空二维数组grid。

> 一个岛屿是由一些相邻的 1 (代表土地) 构成的组合，这里的「相邻」要求两个 1 必须在水平或者竖直方向上相邻。你可以假设 grid 的四个边缘都被 0（代表水）包围着。

> 找到给定的二维数组中最大的岛屿面积。(如果没有岛屿，则返回面积为 0 。)

**解析**：

```python
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        def dfs(grid, r, c):
            if not (0 <= r < len(grid) and 0 <= c < len(grid[0])):
                return 0
            elif grid[r][c] == 0:
                return 0
            elif grid[r][c] != 1:  # grid[r][c] == 2
                return 0
            grid[r][c] = 2
            return 1 + dfs(grid, r-1, c) + dfs(grid, r+1, c) + dfs(grid, r, c-1) + dfs(grid, r, c+1)

        res = 0
        for row in range(len(grid)):
            for col in range(len(grid[0])):
                if grid[row][col] == 1:
                    res =  max(res, dfs(grid, row, col))

        return res
```


## 岛屿的周长
> [Leetcode 463. 岛屿的周长](https://leetcode-cn.com/problems/island-perimeter/)

> 给定一个包含 0 和 1 的二维网格地图，其中 1 表示陆地，0表示水域。

> 网格中的格子水平和垂直方向相连（对角线方向不相连）。整个网格被水完全包围，但其中恰好有一个岛屿（或者说，一个或多个表示陆地的格子相连组成的岛屿）。

> 岛屿中没有“湖”（“湖” 指水域在岛屿内部且不和岛屿周围的水相连）。格子是边长为 1 的正方形。网格为长方形，且宽度和高度均不超过 100 。计算这个岛屿的周长。

**解析**：

```python
class Solution:
    def islandPerimeter(self, grid: List[List[int]]) -> int:
        def dfs(grid, r, c):
            if not (0 <= r < len(grid) and 0 <= c < len(grid[0])):
                return 1
            elif grid[r][c] == 0:
                return 1
            elif grid[r][c] != 1:  # grid[r][c] == 2
                return 0
            grid[r][c] = 2
            return dfs(grid, r-1, c) + dfs(grid, r+1, c) + dfs(grid, r, c-1) + dfs(grid, r, c+1)

        res = 0
        for row in range(len(grid)):
            for col in range(len(grid[0])):
                if grid[row][col] == 1:
                    res = dfs(grid, row, col)
        return res
```


## 被围绕的区域
> [Leetcode 130. 被围绕的区域](https://leetcode-cn.com/problems/surrounded-regions/)

> 给定一个二维的矩阵，包含 'X' 和 'O'（字母 O）。

> 找到所有被 'X' 围绕的区域，并将这些区域里所有的 'O' 用 'X' 填充。

> 示例:
```
X X X X
X O O X
X X O X
X O X X
```

运行你的函数后，矩阵变为：
```
X X X X
X X X X
X X X X
X O X X
```

> 解释:

> 被围绕的区间不会存在于边界上，换句话说，任何边界上的 'O' 都不会被填充为 'X'。 任何不在边界上，或不与边界上的 'O' 相连的 'O' 最终都会被填充为 'X'。如果两个元素在水平或垂直方向相邻，则称它们是“相连”的。

**解析**：标记边界的'O'及相连的'O'，没有被标记的'O'就是被包围的。

> 参考 [Leetcode官方题解](https://leetcode-cn.com/problems/surrounded-regions/solution/bei-wei-rao-de-qu-yu-by-leetcode-solution/)

```python
class Solution:
    def solve(self, board: List[List[str]]) -> None:
        if not board:
            return
        n, m = len(board), len(board[0])

        def dfs(x, y):
            if not 0 <= x < n or not 0 <= y < m or board[x][y] != 'O':
                return
            board[x][y] = "A"
            dfs(x + 1, y)
            dfs(x - 1, y)
            dfs(x, y + 1)
            dfs(x, y - 1)
        
        for i in range(n):
            dfs(i, 0)
            dfs(i, m - 1)
        
        for i in range(m - 1):
            dfs(0, i)
            dfs(n - 1, i)
        
        for i in range(n):
            for j in range(m):
                if board[i][j] == "A":
                    board[i][j] = "O"
                elif board[i][j] == "O":
                    board[i][j] = "X"
```


## 单词搜索
> [Leetcode 79. 单词搜索](https://leetcode-cn.com/problems/word-search/)

> 给定一个二维网格和一个单词，找出该单词是否存在于网格中。

> 单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

> 示例:
```
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

给定 word = "ABCCED", 返回 true
给定 word = "SEE", 返回 true
给定 word = "ABCB", 返回 false
```

**解析**：回溯。

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        def dfs(board, x, y, m, n, word, index, marked):
            if board[x][y] != word[index]:
                return False
            if index == len(word) - 1:
                return True
            # 先占住这个位置，搜索不成功的话，要释放掉
            marked[x][y] = True
            for direction in [[0, -1], [0, 1], [-1, 0], [1, 0]]:
                new_x = x + direction[0]
                new_y = y + direction[1]
                # 如果这一次 search word 成功的话，就返回
                if 0 <= new_x < m and 0 <= new_y < n and \
                        not marked[new_x][new_y] and \
                        dfs(board, new_x, new_y, m, n, word, index + 1, marked):
                    return True
            marked[x][y] = False  # 状态重置

        m = len(board)
        n = len(board[0])
        marked = [[False for _ in range(n)] for _ in range(m)]
        for i in range(m):
            for j in range(n):
                # 对每一个格子都从头开始搜索
                if dfs(board, i, j, m, n, word, 0, marked):
                    return True
        return False
```


## 全排列
> [Leetcode 46. 全排列](https://leetcode-cn.com/problems/permutations/)

> 给定一个 **没有重复** 数字的序列，返回其所有可能的全排列。

> 示例:
```
输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

**解析**：回溯。先画树形图，画图能帮助我们想清楚递归结构，想清楚如何剪枝。

**强烈推荐阅读参考文章**。

> 参考 [回溯算法入门级详解 + 练习（持续更新）](https://leetcode-cn.com/problems/permutations/solution/hui-su-suan-fa-python-dai-ma-java-dai-ma-by-liweiw/)

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        def dfs(nums, size, used, path, depth, res):
            if depth == size:
                res.append(path[:])
                return
            for i in range(size):
                if not used[i]:
                    used[i] = True
                    path.append(nums[i])
                    dfs(nums, size, used, path, depth + 1, res)
                    used[i] = False
                    path.pop()

        size = len(nums)
        if len(nums) == 0:
            return []
        used = [False for _ in range(size)]
        res = []
        dfs(nums, size, used, [], 0, res)
        return res
```


## 全排列 II
> [Leetcode 47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/)

> 给定一个可包含重复数字的序列，返回所有不重复的全排列。

> 示例:

```
输入: [1,1,2]
输出:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```

**解析**：画树形图。

> 参考 [回溯搜索 + 剪枝](https://leetcode-cn.com/problems/permutations-ii/solution/hui-su-suan-fa-python-dai-ma-java-dai-ma-by-liwe-2/)

```python
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        def dfs(nums, size, used, path, depth, res):
            if depth == size:
                res.append(path[:])
                return
            for i in range(size):
                if not used[i]:
                    if i > 0 and nums[i] == nums[i - 1] and not used[i - 1]:
                        continue
                    used[i] = True
                    path.append(nums[i])
                    dfs(nums, size, used, path, depth + 1, res)
                    used[i] = False
                    path.pop()

        size = len(nums)
        if size == 0:
            return []
        nums.sort()
        used = [False] * len(nums)
        res = []
        dfs(nums, size, used, [], 0, res)
        return res
```



## 组合总数
> [Leetcode 39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)

> 给定一个**无重复元素**的数组`candidates`和一个目标数`target`，找出`candidates`中所有可以使数字和为`target`的组合。

> `candidates`中的数字可以无限制重复被选取。

> 说明：
> - 所有数字（包括`target`）都是正整数。
> - 解集不能包含重复的组合。

> 示例 1：
```
输入：candidates = [2,3,6,7], target = 7,
所求解集为：
[
  [7],
  [2,2,3]
]
```

**解析**：关键是如何去重。画树形图，从每一层的第2个结点开始，都不能再搜索产生同一层结点已经使用过的 candidate 里的元素。

> 参考 [回溯算法 + 剪枝（回溯经典例题详解）](https://leetcode-cn.com/problems/combination-sum/solution/hui-su-suan-fa-jian-zhi-python-dai-ma-java-dai-m-2/)

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        def dfs(nums, size, begin, target, path, res):
            if target < 0:
                return
            if target == 0:
                res.append(path[:])
                return 
            for i in range(begin, size):
                path.append(nums[i])
                dfs(nums, size, i, target - nums[i], path, res)
                path.pop()
        
        size = len(candidates)
        if size == 0:
            return []
        res = []
        dfs(candidates, size, 0, target, [], res)
        return res
```

对数组进行排序，使用剪枝加速：
```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        def dfs(nums, size, begin, target, path, res):
            if target == 0:
                res.append(path[:])
                return 
            for i in range(begin, size):
                if nums[i] > target:
                    break
                path.append(nums[i])
                dfs(nums, size, i, residue - nums[i], path, res)
                path.pop()
        
        size = len(candidates)
        if size == 0:
            return []
        res = []
        candidates.sort()
        dfs(candidates, size, 0, target, [], res)
        return res
```


## 组合总数 II
> [Leetcode 40. 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/)

> 给定一个数组`candidates`和一个目标数`target`，找出`candidates`中所有可以使数字和为`target`的组合。

> `candidates`中的每个数字在每个组合中只能使用一次。

> 说明：
> - 所有数字（包括`target`）都是正整数。
> - 解集不能包含重复的组合。

> 示例 1:
```
输入: candidates = [10,1,2,7,6,1,5], target = 8,
所求解集为:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```

**解析**：画树形图。

> [回溯算法 + 剪枝（Java、Python）](https://leetcode-cn.com/problems/combination-sum-ii/solution/hui-su-suan-fa-jian-zhi-python-dai-ma-java-dai-m-3/)

```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        def dfs(nums, size, begin, target, path, res):
            if target == 0:
                res.append(path[:])
                return 
            for i in range(begin, size):
                if nums[i] > target:
                    break
                if i > begin and nums[i] == nums[i - 1]:
                    continue
                path.append(nums[i])
                dfs(nums, size, i + 1, target - nums[i], path, res)
                path.pop()
        
        size = len(candidates)
        if size == 0:
            return []
        res = []
        candidates.sort()
        dfs(candidates, size, 0, target, [], res)
        return res
```


## 组合
> [Leetcode 77. 组合](https://leetcode-cn.com/problems/combinations/)

> 给定两个整数 n 和 k，返回 1 ... n 中所有可能的 k 个数的组合。

> 示例:
```
输入: n = 4, k = 2
输出:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

**解析**：
> [回溯算法 + 剪枝（Java）](https://leetcode-cn.com/problems/combinations/solution/hui-su-suan-fa-jian-zhi-python-dai-ma-java-dai-ma-/)

1、模仿上面的代码，使用数组
```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        def dfs(nums, size, begin, k, path, depth, res):
            if depth == k:
                res.append(path[:])
                return
            for i in range(begin, size):
                path.append(nums[i])
                dfs(nums, size, i + 1, k, path, depth + 1, res)
                path.pop()
        
        nums = list(range(1, n + 1))
        res = []
        dfs(nums, len(nums), 0, k, [], 0, res)
        return res
```

2、不用数组
```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        def dfs(begin, n, k, path, depth, res):
            if depth == k:
                res.append(path[:])
                return
            for i in range(begin, n + 1):
                path.append(i)
                dfs(i + 1, n, k, path, depth + 1, res)
                path.pop()
        
        res = []
        dfs(1, n, k, [], 0, res)
        return res
```

事实上，如果`n = 7, k = 4`，从 5 开始搜索就已经没有意义了，这是因为：即使把 5 选上，后面的数只有 6 和 7，一共就 3 个候选数，凑不出 4 个数的组合。因此，搜索起点有上界，可以使用剪枝技术，避免不必要的遍历，加快算法。

剪枝的具体做法是把循环条件改成：
```python
# 1、使用数组，其中的begin表示数组下标，初始为0
for i in range(begin, size - (k - depth) + 1):
# 2、不用数组，其中的begin表示数字，初始为1
for i in range(begin, n - (k - depth) + 2):
```


## 子集
> [Leetcode 78. 子集](https://leetcode-cn.com/problems/subsets/)

> 给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。

> 说明：解集不能包含重复的子集。

> 示例:
```
输入: nums = [1,2,3]
输出:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

**解析**：参考 77 题-解析1，并使用了剪枝。

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        def dfs(nums, size, begin, k, path, depth, res):
            if depth == k:
                res.append(path[:])
                return
            for i in range(begin, size - (k - depth) + 1): # 剪枝
                path.append(nums[i])
                dfs(nums, size, i + 1, k, path, depth + 1, res)
                path.pop()
        
        res = [[]]
        for k in range(1, len(nums) + 1):
            dfs(nums, len(nums), 0, k, [], 0, res)
        return res
```


## 子集 II
> [Leetcode 90. 子集 II](https://leetcode-cn.com/problems/subsets-ii/)

> 给定一个可能包含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。

> 说明：解集不能包含重复的子集。

> 示例:
```
输入: [1,2,2]
输出:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```

**解析**：参考 78 题，剪枝技巧同 47 题、40 题。

```python
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        def dfs(nums, size, begin, k, path, depth, res):
            if depth == k:
                res.append(path[:])
                return
            for i in range(begin, size - (k - depth) + 1):
                if i > begin and nums[i] == nums[i - 1]:
                    continue
                path.append(nums[i])
                dfs(nums, size, i + 1, k, path, depth + 1, res)
                path.pop()
            
        res = [[]]
        nums.sort()
        for k in range(1, len(nums) + 1):
            dfs(nums, len(nums), 0, k, [], 0, res)
        return res
```


## 第k个排列
> [Leetcode 60. 第k个排列](https://leetcode-cn.com/problems/permutation-sequence/)

> 给出集合 [1,2,3,…,n]，其所有元素共有 n! 种排列。

> 按大小顺序列出所有排列情况，并一一标记，当 n = 3 时, 所有排列如下：
> 1. "123"
> 2. "132"
> 3. "213"
> 4. "231"
> 5. "312"
> 6. "321"

> 给定 n 和 k，返回第 k 个排列。

> 说明：
> - 给定 n 的范围是 [1, 9]。
> - 给定 k 的范围是 [1, n!]。

**解析**：

1、参考 46 题，超时

```python
class Solution:
    def getPermutation(self, n: int, k: int) -> str:
        def dfs(nums, size, k, used, path, depth, res):
            if depth == size:
                res[0] += 1
                if res[0] == k:
                    res[1] = "".join(str(d) for d in path)
                return
            for i in range(size):
                if not used[i]:
                    used[i] = True
                    path.append(nums[i])
                    dfs(nums, size, k, used, path, depth + 1, res)
                    used[i] = False
                    path.pop()
                    if res[0] == k: # 得到结果即返回
                        break
                        
        nums = list(range(1, n + 1))
        used = [False for _ in range(n)]
        res = [0, ""]
        dfs(nums, n, k, used, [], 0, res)
        return res[1]
```

2、画树形图，所求排列一定在叶子结点处得到，每个分支下的叶结点个数可求，所以不必求出所有的全排列。

> 参考 [深度优先遍历 + 剪枝、有序数组模拟](https://leetcode-cn.com/problems/permutation-sequence/solution/hui-su-jian-zhi-python-dai-ma-java-dai-ma-by-liwei/)

==TODO==


## 复原IP地址
> [Leetcode 93. 复原IP地址](https://leetcode-cn.com/problems/restore-ip-addresses/)

> 给定一个只包含数字的字符串，复原它并返回所有可能的 IP 地址格式。

> 有效的 IP 地址 正好由四个整数（每个整数位于 0 到 255 之间组成，且不能含有前导 0），整数之间用 '.' 分隔。

> 例如："0.1.2.201" 和 "192.168.1.1" 是 有效的 IP 地址，但是 "0.011.255.245"、"192.168.1.312" 和 "192.168@1.1" 是 无效的 IP 地址。

> 示例 5：
```
输入：s = "101023"
输出：["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]
```

**解析**：

==TODO==


## 电话号码的字母组合
> [Leetcode 17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

> 给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。

> 给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

> 示例:
```
输入："23"
输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

**解析**：经典回溯。

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if not  digits:
            return [] 

        phone_map = {
            "2": "abc",
            "3": "def",
            "4": "ghi",
            "5": "jkl",
            "6": "mno",
            "7": "pqrs",
            "8": "tuv",
            "9": "wxyz",
        }

        def dfs(digits, index, phone_map, combination, res):
            if index == len(digits):
                res.append("".join(combination))
            else:
                digit = digits[index]
                for letter in phone_map[digit]:
                    combination.append(letter)
                    dfs(digits, index + 1, phone_map, combination, res)
                    combination.pop()
        
        res = []
        dfs(digits, 0, phone_map, [], res)
        return res
```



# 数学
## 圆圈中最后剩下的数字
> [Leetcode 剑指 Offer 62. 圆圈中最后剩下的数字](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)

> 0,1,,n-1这n个数字排成一个圆圈，从数字0开始，每次从这个圆圈里删除第m个数字。求出这个圆圈里剩下的最后一个数字。

> 例如，0、1、2、3、4这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。

**解析**：

> 参考 [LeetCode-Solution](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/solution/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-by-lee/)

> 参考 [约瑟夫环——公式法（递推公式）](https://blog.csdn.net/u011500062/article/details/72855826)

1、使用数组模拟，超时。


2、数学+递归
```python
# Python 默认的递归深度不够，需要手动设置
sys.setrecursionlimit(100000)

class Solution:
    def lastRemaining(self, n: int, m: int) -> int:
        def f(n, m):
            if n == 0:
                return 0
            x = f(n - 1, m)
            return (m + x) % n
        
        return f(n, m)
```

3、递推公式
```python
class Solution:
    def lastRemaining(self, n: int, m: int) -> int:
        res = 0
        for i in range(2, n + 1):
            res = (res + m) % i
        return res
```


## 阶乘后的零
> [Leetcode 172. 阶乘后的零](https://leetcode-cn.com/problems/factorial-trailing-zeroes/)

**解析**：

```python
class Solution:
    def trailingZeroes(self, n: int) -> int:
        res = 0
        while n >= 5:
            res += n // 5
            n //= 5
        
        return res
```


## 数字 1 的个数
> [Leetcode 233. 数字 1 的个数](https://leetcode-cn.com/problems/number-of-digit-one/)

> 给定一个整数 n，计算所有小于等于 n 的非负整数中数字 1 出现的个数。

> 示例:
```
输入: 13
输出: 6 
解释: 数字 1 出现在以下数字中: 1, 10, 11, 12, 13 。
```

**解析**：

> 参考 [Leetcode官方题解](https://leetcode-cn.com/problems/number-of-digit-one/solution/shu-zi-1-de-ge-shu-by-leetcode/)

```python
class Solution:
    def countDigitOne(self, n: int) -> int:
        if n <= 0:
            return 0
        count = 0
        i = 1
        while n // i != 0:
            high = n // (10 * i)
            cur = (n // i) % 10
            low = n - (n // i) * i
            if cur == 0:
                count += high * i
            elif cur == 1:
                count += high * i + ( low + 1)
            else:
                count += (high + 1) * i
            i = i * 10

        return count
```


## 幂集
> [Leetcode 面试题 08.04. 幂集](https://leetcode-cn.com/problems/power-set-lcci/)

> 幂集。编写一种方法，返回某集合的所有子集。集合中不包含重复的元素。

> 说明：解集不能包含重复的子集。

**解析**：

1、使用位图，每个二进制位是否选择一个数字，0表示不选择，1表示选择。
```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        n = len(nums)
        output = []
        
        for i in range(2**n, 2**(n + 1)):
            # generate bitmask, from 0..00 to 1..11
            bitmask = bin(i)[3:]
            output.append([nums[j] for j in range(n) if bitmask[j] == '1'])
        
        return output
```

2、DFS，与Leetcode 78相同。代码略。


## 整数拆分
> [Leetcode 343. 整数拆分](https://leetcode-cn.com/problems/integer-break/)

> [Leetcode 剑指 Offer 14-I. 剪绳子](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)

> 给定一个正整数 n，将其拆分为**至少**两个正整数的和，并使这些整数的乘积最大化。 返回你可以获得的最大乘积。

**解析**：把整数拆分出较多的3，注意4需要拆分成两个2。

> 参考 [Leetcode-Krahets](https://leetcode-cn.com/problems/integer-break/solution/343-zheng-shu-chai-fen-tan-xin-by-jyd/)

```python
class Solution:
    def integerBreak(self, n: int) -> int:
        if n <= 3:
            return n - 1
        if n % 3 == 0:
            return 3 ** (n//3)
        if n % 3 == 1:
            return 3 ** (n//3 - 1) * 2 * 2
        return 3 ** (n//3) * 2
```

## 用 Rand7() 实现 Rand10()
> [Leetcode 470. 用 Rand7() 实现 Rand10()](https://leetcode-cn.com/problems/implement-rand10-using-rand7/)

> 已有方法 rand7 可生成 1 到 7 范围内的均匀随机整数，试写一个方法 rand10 生成 1 到 10 范围内的均匀随机整数。

> 不要使用系统的 Math.random() 方法。

**解析**：

```python
class Solution:
    def rand10(self):
        res = 49
        while res > 40:
            res = (rand7() - 1) * 7 + rand7() # 1 - 40
        return (res - 1) % 10 + 1
```

用 Rand3() 实现 Rand5()：
```python
class Solution:
    def rand5(self):
        res = 9
        while res > 5:
            res = (rand3() - 1) * 3 + rand3() # 1 - 5
        return res
```

用 Rand5() 实现 Rand7():
```
class Solution:
    def rand7(self):
        res = 9
        while res > 21:
            res = (rand5() - 1) * 5 + rand5() # 1 - 21
        return (res - 1) % 7 + 1
```


## 矩形面积
> [Leetcode 223. 矩形面积](https://leetcode-cn.com/problems/rectangle-area/)

> 在二维平面上计算出两个由直线构成的矩形重叠后形成的总面积。

> 每个矩形由其左下顶点和右上顶点坐标表示，如图所示。

**解析**：

```python
class Solution:
    def computeArea(self, A: int, B: int, C: int, D: int, E: int, F: int, G: int, H: int) -> int:
        area1 = (C - A) * (D - B)
        area2 = (G - E) * (H - F)
        if E >= C or F >= D or A >= G or B >= H:
            return area1 + area2
        cross_area = (min(C, G) - max(A, E)) * (min(D, H) - max(B, F))
        return area1 + area2 - cross_area
```


## 丑数 II
> [Leetcode 264. 丑数 II](https://leetcode-cn.com/problems/ugly-number-ii/)

> 编写一个程序，找出第 n 个丑数。

> 丑数就是质因数只包含 2, 3, 5 的正整数。

> 示例:
```
输入: n = 10
输出: 12
解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。
```

**解析**：

```python
class Solution:
    def nthUglyNumber(self, n: int) -> int:
        num_list = [1]
        i2 = i3 = i5 = 0
        while len(num_list) < n:
            num2 , num3, num5 = num_list[i2] * 2, num_list[i3] * 3, num_list[i5] * 5
            num = min(num2, num3, num5)
            if num == num2:
                i2 += 1
            if num == num3:
                i3 += 1
            if num == num5:
                i5 += 1
            num_list.append(num)
        return num_list[-1]
```



# 位运算

## 二进制中1的个数

> [Leetcode 191. 位1的个数](https://leetcode-cn.com/problems/number-of-1-bits/)
>
> [剑指 Offer 15. 二进制中1的个数](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)

> 请实现一个函数，输入一个整数（以二进制串形式），输出该数二进制表示中 1 的个数。例如，把 9 表示成二进制是 1001，有 2 位是 1。因此，如果输入 9，则该函数输出 2。

**解析**：

解法1

解法2

解法3



# 设计
## LRU缓存机制
> [Leetcode 146. LRU缓存机制](https://leetcode-cn.com/problems/lru-cache/)

> 运用你所掌握的数据结构，设计和实现一个 [LRU (最近最少使用)](https://baike.baidu.com/item/LRU) 缓存机制。它应该支持以下操作： 获取数据 get 和 写入数据 put 。

> 获取数据 get(key) - 如果关键字 (key) 存在于缓存中，则获取关键字的值（总是正数），否则返回 -1。

> 写入数据 put(key, value) - 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字/值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。

**解析**：可以使用有序字典。但是，在面试中，面试官一般会期望面试者能够自己实现一个简单的双向链表，而不是使用语言自带的、封装好的数据结构，哈希表可以使用语言自带的。

> 参考 [Leetcode官方题解](https://leetcode-cn.com/problems/lru-cache/solution/lruhuan-cun-ji-zhi-by-leetcode-solution/)

1、使用有序字典。
```python
class LRUCache:
    def __init__(self, capacity: int):
        from collections import OrderedDict
        self.capacity = capacity
        self.cache = OrderedDict()
        
    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1
        self.cache.move_to_end(key)
        return self.cache[key]
        
    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            self.cache.move_to_end(key)
        self.cache[key] = value
        if len(self.cache) > self.capacity:
            self.cache.popitem(last=False)
```

2、使用自带的哈希表和自己实现的双向链表。
```python
class DLinkedNode:
    def __init__(self, key=None, value=None):
        self.key = key
        self.value = value
        self.prev = None
        self.next = None

class LRUCache:
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.size = 0
        self.cache = dict()
        # 使用伪头部和伪尾部节点    
        self.head = DLinkedNode()
        self.tail = DLinkedNode()
        self.head.next = self.tail
        self.tail.prev = self.head
    
    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1
        # 如果 key 存在，先通过哈希表定位，再移到头部
        node = self.cache[key]
        self.moveToHead(node)
        return node.value
        
    def put(self, key: int, value: int) -> None:
        if key not in self.cache:
            # 如果 key 不存在，创建一个新的节点
            node = DLinkedNode(key, value)
            # 添加进哈希表
            self.cache[key] = node
            # 添加至双向链表的头部
            self.addToHead(node)
            self.size += 1
            if self.size > self.capacity:
                # 如果超出容量，删除双向链表的尾部节点
                removed = self.removeTail()
                # 删除哈希表中对应的项
                self.cache.pop(removed.key)
                self.size -= 1
        else:
            # 如果 key 存在，先通过哈希表定位，再修改 value，并移到头部
            node = self.cache[key]
            node.value = value
            self.moveToHead(node)
    
    def addToHead(self, node):
        node.prev = self.head
        node.next = self.head.next
        self.head.next.prev = node
        self.head.next = node
    
    def removeNode(self, node):
        node.prev.next = node.next
        node.next.prev = node.prev
        
    def moveToHead(self, node):
        self.removeNode(node)
        self.addToHead(node)
        
    def removeTail(self):
        node = self.tail.prev
        self.removeNode(node)
        return node
```


## 数据流的中位数
> [Leetcode 295. 数据流的中位数](https://leetcode-cn.com/problems/find-median-from-data-stream/)

> 中位数是有序列表中间的数。如果列表长度是偶数，中位数则是中间两个数的平均值。

> 例如，

> [2,3,4] 的中位数是 3

> [2,3] 的中位数是 (2 + 3) / 2 = 2.5

> 设计一个支持以下两种操作的数据结构：

> - void addNum(int num) - 从数据流中添加一个整数到数据结构中。
> - double findMedian() - 返回目前所有元素的中位数。

**解析**：使用堆。

1、保持数组有序，添加数据时使用二分搜索。
```python
class MedianFinder:
    def __init__(self):
        self.nums = []
        
    def addNum(self, num: int) -> None:
        import bisect
        bisect.insort(self.nums, num)
        
    def findMedian(self) -> float:
        l = len(self.nums)
        num1 = self.nums[(l - 1) // 2]
        num2 = self.nums[l // 2]
        return (num1 + num2) / 2
```

复杂度分析：
- 时间复杂度：二分搜索花费`O(log n)`，插入元素可能需要花费`O(n)`，总共是`O(n)`。
- 空间复杂度：`O(n)`。

2、使用两个堆，一个最大堆，一个最小堆。最大堆用于存储输入数字中较小的一半，方便获得其中的最大值；最小堆用于存储输入数字的较大的一半，方便获得其中的最小值。
```python
from heapq import *

class MedianFinder:
    def __init__(self):
        self.min_heap = []  # keep the larger half numbers
        self.max_heap = []  # keep the smaller half numbers
        
    def addNum(self, num: int) -> None:
        heappush(self.min_heap, num)
        target = heappop(self.min_heap)
        heappush(self.max_heap, -target)
        if len(self.min_heap) < len(self.max_heap):
            target = heappop(self.max_heap)
            heappush(self.min_heap, -target)
            
    def findMedian(self) -> float:
        if len(self.min_heap) == len(self.max_heap):
            return (self.min_heap[0] - self.max_heap[0]) / 2
        return self.min_heap[0]
```

复杂度分析：
- 时间复杂度：最坏情况下，从顶部有三个堆插入和两个堆删除。每一个都需要花费`O(log n)`，所以最终是`O(log n)`。
- 空间复杂度：`O(n)`。
---
title:       "代码随想录Leetcode Day2 Array Part 2"
subtitle:    ""
description: "209. Minimum Size Subarray Sum, 59. Spiral Matrix II, Interval Sum, Developers Purchase Land"
date:        "2025-01-10T00:54:38Z"
author:      "Hao Xu"
image:       ""
tags:        ["tag1", "tag2"]
categories:  ["Tech" ]
draft:       false
---

# [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/description/)

## Description

Given an array of positive integers `nums` and a positive integer `target`, return the **minimal length** of a subarray whose sum is greater than or equal to `target`. If there is no such subarray, return `0` instead.

**Example 1**:

```
Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: The subarray [4,3] has the minimal length under the problem constraint.
```

**Example 2**:

```
Input: target = 4, nums = [1,4,4]
Output: 1
```

**Example 3**:

```
Input: target = 11, nums = [1,1,1,1,1,1,1,1]
Output: 0
```

**Constraints**:

- `1 <= target <= 109`
- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 104`

Follow up: If you have figured out the `O(n)` solution, try coding another solution of which the time complexity is `O(n log(n))`.

## Brute Force

最直接的方法就是通过两重循环，枚举所有的子数组，然后计算子数组的和，如果大于等于target，就更新最小长度。  
但是这种方法的时间复杂度是$O(n^2)$，不符合题目要求。

具体C++代码如下：  

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        // int min_len = nums.size();
        int min_len = INT_MAX;
        for (int i = 0; i < nums.size(); i++) {
            int sum = 0;
            for (int j = i; j < nums.size(); j++) {
                sum += nums[j];
                if (sum >= target) {
                    min_len = min_len < j - i + 1 ? min_len : j - i + 1;
                    // min_len = std::min(min_len, j-i+1);
                    break;
                }
            }
            
        }
        return min_len == INT_MAX ? 0 : min_len;
    }
};
```

## Two Pointers (Sliding Window)

通过双指针实现滑动窗口，可以将时间复杂度降低到$O(n)$。  

**【注意】**：  
该题目的数组元素都是正整数，滑动窗口的和是递增的，所以可以通过滑动窗口的和来判断窗口是否需要移动左指针。  
外部`for`循环针对右指针`right`，内部`while`循环针对左指针`left`。  
注意理解滑动窗口的思想和原理。其关键在于数组元素是正整数导致的窗口和的递增性。  

你可能会想：`for`循环里面还有一个`while`循环，时间复杂度不是 $O(n^2)$ 吗？之所以还是 $O(n)$ ，是因为右指针`right`可以移动 n 次，左指针`left`也可以移动 次。对于外循环的每次迭代，内循环不会运行 n 次。滑动窗口保证最多 2n 次窗口迭代。这就是所谓的摊销分析 ([Amortized analysis](https://en.wikipedia.org/wiki/Amortized_analysis)) - 尽管`for`循环内迭代的最坏情况是$O(n)$，但当您考虑整个运行时时，它的平均值为$O(1)$算法。

具体C++代码如下：

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int min_len = INT_MAX;
        int left = 0;
        int sum = 0;
        for (int right = 0; right < nums.size(); right++) {
            sum += nums[right];
            while (sum >= target) {
                min_len = min_len < right - left + 1 ? min_len : right - left + 1;
                // min_len = std::min(min_len, right-left+1);
                sum -= nums[left++];
            }
        }
        return min_len == INT_MAX ? 0 : min_len;
    }
};
```

## Binary Search

对于$O(n\ log\ n)$的解法，可以通过二分查找的方法实现。  
大致思路是先计算前缀和，然后通过二分查找找到对于每个起点`i`，满足`sum[j] - sum[i] >= target`的最小`j`。  
同样需要注意的是，该题目的数组元素都是正整数，前缀和是递增的，所以可以通过前缀和来判断二分查找的方向。

具体C++代码如下：

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int min_len = INT_MAX;
        vector<int> prefix_sum(nums.size() + 1, 0);

        for (int i = 1; i <= nums.size(); i++)    prefix_sum[i] = prefix_sum[i - 1] + nums[i - 1];

        for (int i = 0; i < nums.size(); i++) {
            int left = i + 1;
            int right = nums.size();
            int result = -1;
            while (left <= right) {
                int middle = left + ((right - left) >> 1);
                if (prefix_sum[middle] - prefix_sum[i] < target) {
                    left = middle + 1;
                } else {
                    right = middle - 1;
                    result = middle;
                }
            }
            if (result != -1)   min_len = min_len < result - i ? min_len : result - i;
        }
        return min_len == INT_MAX ? 0 : min_len;
    }
};
```

# [59. Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii/description/)

## Description

Given a positive integer `n`, generate an `n x n` `matrix` filled with elements from 1 to $n^2$ in spiral order.

**Example 1**:

```
Input: n = 3
Output: [[1,2,3],[8,9,4],[7,6,5]]
```

**Example 2**:

```
Input: n = 1
Output: [[1]]
```

**Constraints**:

- `1 <= n <= 20`

---
title:       "代码随想录Leetcode Day2 Array Part 2"
subtitle:    ""
description: "209. Minimum Size Subarray Sum, 59. Spiral Matrix II, Interval Sum, Developers Purchase Land"
date:        "2025-01-10T00:54:38Z"
author:      "Hao Xu"
image:       ""
tags:        ["Leetcode", "Job"]
categories:  ["Tech" , "Diary"]
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
![Example 1](/img/2025-01-10-Leetcode-Day2-Array2/spiral_example.jpg)

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

## 解题思路

这题主要是模拟螺旋矩阵的生成过程，需要注意的是边界条件的处理。核心思想是解码底层模式。这可以通过模拟模式并找到适用于任何给定 n 的通用表示来完成。

Traverse Layer by Layer in Spiral Form
螺旋式逐层遍历

### Intuition

如果我们尝试为给定的 $n$ 构建一种模式，我们会观察到该模式在完成围绕矩阵的一次循环遍历后会重复。我们将这种循环遍历称为层。我们从外层开始遍历，并在每次迭代中向内层移动。

![Spiral Layer](/img/2025-01-10-Leetcode-Day2-Array2/spiral_layers.png)

### Algorithm  算法

Let's devise an algorithm for the spiral traversal:  
让我们设计一个螺旋遍历的算法：

- We can observe that, for any given $n$, the total number of layers is given by :  
- 我们可以观察到，对于任何给定的 $n$ ，总层数由下式给出：$$layers = \lceil \frac{n+1}{2} \rceil$$

This works for both even and odd $n$.  
这适用于偶数和奇数 n 。

*Example*  
*例子*

For $n=3, layers=2$  
For $n=6$, total $layers=3$

- Also, for each layer, we traverse in at most 4 directions :  
- 此外，对于每一层，我们最多遍历 4 个方向：
![Spiral Traversal](/img/2025-01-10-Leetcode-Day2-Array2/spiral_traverse.png)

In every direction, either row or column remains constant and other parameter changes (increments/decrements).  
在每个方向上，行或列保持不变，而其他参数发生变化（增量/减量）。

Direction 1: From top left corner to top right corner.  
方向1：从左上角到右上角。  
The row remains constant as $layer$ and column increments from $layer$ to $n−layer−1$  
行保持不变为 $layer$ ，列从 $layer$ 递增到 $n−layer−1$

Direction 2: From top right corner to the bottom right corner.  
方向2：从右上角到右下角。  
The column remains constant as $n−layer−1$ and row increments from $layer+1$ to $n−layer$.
列保持不变为 $n−layer−1$ ，行从 $layer+1$ 到 $n−layer$ 。

Direction 3: From bottom right corner to bottom left corner.  
方向3：从右下角到左下角。  
The row remains constant as $n−layer−1$ and column decrements from $n−layer−2$ to $layer$.
行保持不变为 $n−layer−1$ ，列从 $n−layer−2$ 递减到 $layer$ 。

Direction 4: From bottom left corner to top left corner.  
方向4：从左下角到左上角。  
The column remains constant as $layer$ and column decrements from $n−layer−2$ to $layer+1$.  
该列保持不变为 $layer$ ，并且列从 $n−layer−2$ 递减到 $layer+1$ 。

This process repeats $(n+1)/2$ times until all layers are traversed.  
此过程重复 $(n+1)/2$ 次，直到遍历完所有层。

![Spiral Details](/img/2025-01-10-Leetcode-Day2-Array2/spiral_detailed.png)

### Code

详细的C++代码如下：

```cpp
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> matrix(n, vector<int>(n, 0));
        int num = 1;
        for (int layer = 0; layer < (n+1)/2; layer++){
            for (int ptr = layer; ptr < n-layer; ptr++) {
                matrix[layer][ptr] = num++;
            }
            for (int ptr = layer + 1; ptr < n-layer; ptr++) {
                matrix[ptr][n-layer-1] = num++;
            }
            for (int ptr = n-layer-2; ptr >= layer; ptr--) {
                matrix[n-layer-1][ptr] = num++;
            }
            for (int ptr = n-layer-2; ptr > layer; ptr--) {
                matrix[ptr][layer] = num++;
            }
        }
        return matrix;
    }
};
```

时间复杂度：$O(n^2)$  

# [Kamacoder 58. Interval Sum](https://kamacoder.com/problempage.php?pid=1070)

## Description

**58. 区间和（第九期模拟笔试）**  
给定一个整数数组 Array，请计算该数组在每个指定区间内元素的总和。

**输入描述**  
第一行输入为整数数组 Array 的长度 n，接下来 n 行，每行一个整数，表示数组的元素。随后的输入为需要计算总和的区间下标：a，b （b > = a），直至文件结束。

**输出描述**  
输出每个指定区间内元素的总和。

**输入示例**  

```
5
1
2
3
4
5
0 1
1 3
```

**输出示例**  

```
3
9
```

**数据范围**：

- `0 < n <= 100000`

# [Kamacoder 44. Developers Purchase Land](https://kamacoder.com/problempage.php?pid=1044)

## Description

# Reference

[代码随想录](https://programmercarl.com/)

[LeetCode 209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/solutions/823106/minimum-size-subarray-sum)

[LeetCode 59. Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii/description/)

[LeetCode 59. Spiral Matrix II Solution](https://leetcode.com/problems/spiral-matrix-ii/solutions/823106/spiral-matrix-ii)

[一入循环深似海 | LeetCode：59.螺旋矩阵II](https://www.bilibili.com/video/BV1SL4y1N7mV/)

[kamacoder 58. Interval Sum](https://kamacoder.com/problempage.php?pid=1070)

[kamacoder 44. Developers Purchase Land](https://kamacoder.com/problempage.php?pid=1044)

---
title:       "代码随想录Leetcode Day1 Array Part 1"
subtitle:    ""
description: "704. Binary Search, 27. Remove Element, 977. Square of a Sorted Array"
date:        "2025-01-09T16:17:11Z"
author:      "Hao Xu"
image:       ""
tags:        ["Leetcode", "Job"]
categories:  ["Tech", "Diary" ]
draft:       false
---

# Array

数组是存放在连续内存空间上的相同类型数据的集合。

数组的特点：  

- 数组下标都是从0开始的。
- 数组内存空间的地址是连续的

正是因为数组在内存空间的地址是连续的，所以我们在删除或者增添元素的时候，就难免要移动其他元素的地址。

# [704. Binary Search](https://leetcode.com/problems/binary-search/description/)

## Description

Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`.  
If `target` exists, then return its index. Otherwise, return `-1`.  
You must write an algorithm with `O(log n)` runtime complexity.

**Example 1**:  

```
Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4
```

**Example 2**:  

```
Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1
```

**Constraints**:  

- `1 <= nums.length <= 104`
- `-104 < nums[i], target < 104`
- All the integers in `nums` are **unique**.
- `nums` is sorted in **ascending** order.

此题可以使用二分查找的条件是数组是有序的且没有重复元素。

二分查找的时间复杂度是$O(log\ n)$，空间复杂度是$O(1)$。

最值得注意的是，二分查找的**边界条件**，区间是左闭右开的，即`[left, right)`或左闭右闭，即`[left, right]`。  
还有一点就是middle的取值，如果直接使用`(left + right) / 2`，可能会导致整数溢出，所以应当使用`left + (right - left) / 2`。此外，对于除2的操作，可以使用`>> 1`来代替（位运算符优先级大于`+`、`-`所以不要忘了加括号，即`int middle = left + ((right - left) >> 1);`）。

## 左闭右闭`[left, right]`

通过`array.size() - 1`来获取最后一个元素的下标。最初的区间应当是`[0, array.size() - 1]`。  
由于是闭区间，`left`与`right`可以相等，所以`while`循环的条件是`left <= right`。  
对于边界条件`left`和`right`的更新，应当是`left = middle + 1`和`right = middle - 1`。即检测过后的`middle`不应该再次被包含在区间内。  

具体C++代码如下：

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size()-1;
        while (left <= right) {
            int middle = left + ((right - left) / 2);// 防止right和left太大导致整数溢出
            if (nums[middle] > target)  right = middle - 1;
            else if (nums[middle] < target) left = middle + 1;
            else return middle;
        }
        return -1;
    }
};
```

## 左闭右开`[left, right)`

通过`array.size()`来获取数组长度，其刚好比最后一个元素的下表大一，可用于右开区间的`right`。最初的区间应当是`[0, array.size()]`。  
由于是左闭右开，`left`与`right`不能相等，所以`while`循环的条件是`left < right`。  
对于边界条件`left`和`right`的更新，由于是`left`左闭区间，所以检测过的`middle`不应该再次被包含在区间内，所以`left = middle + 1`。而`right`是开区间，`middle`作为右开区间的`right`不会再次被检测，所以`right = middle`。

具体C++代码如下：  

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size();
        while (left < right) {
            int middle = left + ((right - left) / 2);// 防止right和left太大导致整数溢出
            if (nums[middle] > target)  right = middle;
            else if (nums[middle] < target) left = middle + 1;
            else return middle;
        }
        return -1;
    }
};
```

# [27. Remove Element](https://leetcode.com/problems/remove-element/description/)

## Description

Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in `nums` in-place. The order of the elements may be changed. Then return the number of elements in `nums` which are not equal to `val`.

Consider the number of elements in `nums` which are not equal to `val` be `k`, to get accepted, you need to do the following things:  

- Change the array `nums` such that the first `k` elements of `nums` contain the elements which are not equal to `val`. The remaining elements of `nums` are not important as well as the size of `nums`.
- Return k.

**Custom Judge**:

The judge will test your solution with the following code:  

```
int[] nums = [...]; // Input array
int val = ...; // Value to remove
int[] expectedNums = [...]; // The expected answer with correct length.
                            // It is sorted with no values equaling val.

int k = removeElement(nums, val); // Calls your implementation

assert k == expectedNums.length;
sort(nums, 0, k); // Sort the first k elements of nums
for (int i = 0; i < actualLength; i++) {
    assert nums[i] == expectedNums[i];
}
```

If all assertions pass, then your solution will be **accepted**.

**Example 1**:  

```
Input: nums = [3,2,2,3], val = 3
Output: 2, nums = [2,2,_,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 2.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

**Example 2**:  

```
Input: nums = [0,1,2,2,3,0,4,2], val = 2
Output: 5, nums = [0,1,4,0,3,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums containing 0, 0, 1, 3, and 4.
Note that the five elements can be returned in any order.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

**Constraints**:  

- `0 <= nums.length <= 100`
- `0 <= nums[i] <= 50`
- `0 <= val <= 100`

## Brute Force

暴力解法是通过双层循环，外层循环遍历数组，当遇到val时，内层循环将后面的元素前移。  
需要注意的是当遇到val时，外层循环的下标应当减一，因为内层循环将数组元素前移，所以外层循环的下标减一。  
该解法的时间复杂度是$O(n^2)$，空间复杂度是$O(1)$。

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int size = nums.size();
        for (int i = 0; i < size; i++) {
            if (nums[i] == val) {
                for (int j = i; j < size-1; j++) {
                    nums[j] = nums[j+1];
                }
                i--;
                size--;
            }
        }
        return size;
    }
};
```

## Two Pointers (Fast and Slow Pointers)

双指针法（快慢指针法）： 通过一个快指针和慢指针在一个for循环下完成两个for循环的工作。

定义快慢指针  
快指针：寻找新数组的元素 ，新数组就是不含有目标元素的数组  
慢指针：指向更新 新数组下标的位置

当快指针指向的元素不等于目标元素时，将快指针指向的元素赋值给慢指针指向的位置，然后慢指针指向下一个位置。  
当快指针指向的元素等于目标元素时，快指针指向下一个位置，慢指针不动。

时间复杂度是$O(n)$，空间复杂度是$O(1)$。

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slow = 0;
        for (int fast = 0; fast < nums.size(); fast++) {
            if (nums[fast] != val) {
                nums[slow++] = nums[fast];
            }
        }
        return slow;
    }
};
```

**注意**：`fast < nums.size()`和 `fast ＜=nums.size（）- 1` 没什么区别，那为什么第二个会在空数组时报数组越界的错误？  
`vector`的`size()`函数返回值是无符号整数，空数组时返回了0，再减个一会溢出；变成一个很大的正数，`nums[fast]`会发生`null pointer of type 'int'` 异常。

# [977. Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/description/)

## Description

Given an integer array `nums` sorted in **non-decreasing** order, return an array of **the squares of each number** sorted in non-decreasing order.

**Example 1**:  

```
Input: nums = [-4,-1,0,3,10]
Output: [0,1,9,16,100]
Explanation: After squaring, the array becomes [16,1,0,9,100].
After sorting, it becomes [0,1,9,16,100].
```

**Example 2**:  

```
Input: nums = [-7,-3,2,3,11]
Output: [4,9,9,49,121]
```

**Constraints**:  

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` is sorted in **non-decreasing** order.

**Follow up**: Squaring each element and sorting the new array is very trivial, could you find an `O(n)` solution using a different approach?

## Brute Force

先计算平方后的数组，然后再通过vector容器自带的sort函数排序。
`vector`容器自带的`sort`函数，时间复杂度是$O(n\ log\ n)$，空间复杂度是$O(n)$。

所以该解法的时间复杂度是$O(n + n\ log\ n)$，即$O(n\ log\ n)$。

具体C++代码如下：

```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        for (int i = 0; i < nums.size(); i++) {
            nums[i] = nums[i] * nums[i];
        }
        sort(nums.begin(), nums.end());
        return nums;
    }
};
```

## Two Pointers

由于数组是有序的，所以数组的平方也是有序的。  
最大的平方值一定在数组的两端，所以可以使用双指针法，通过比较两端的平方值，将较大的平方值放在新数组的末尾。

同704. Binary Search一样，双指针法需要注意边界条件应当使用闭区间，即`while`循环的条件是`left <= right`。  

具体C++代码如下：

```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        vector<int> squares(nums.size(), 0);
        int index = nums.size() - 1;
        int left = 0;
        int right = nums.size() - 1;
        while (left <= right) {
        // for (int left = 0, right = nums.size() - 1; left <= right;) {
            if (nums[left] * nums[left] < nums[right] * nums[right]) {
                squares[index--] = nums[right] * nums[right];
                right--;
            } 
            else {
                squares[index--] = nums[left] * nums[left];
                left++;
            }
        }
        return squares;

    }
};
```

其中`while`循环的条件以及相关变量的初始化可以使用`for`循环来代替，来进行简化。

# Reference

[代码随想录](https://programmercarl.com/)

[LeetCode 704. Binary Search](https://leetcode.com/problems/binary-search/description/)

[手把手带你撕出正确的二分法 | 二分查找法 ｜ 二分搜索法 | LeetCode: 704. 二分查找](https://www.bilibili.com/video/BV1fA4y1o715)

[LeetCode 27. Remove Element](https://leetcode.com/problems/remove-element/description/)

[数组中移除元素并不容易！ | LeetCode：27. 移除元素](https://www.bilibili.com/video/BV12A4y1Z7LP/)

[LeetCode 977. Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/description/)

[双指针法经典题目 | LeetCode：977.有序数组的平方](https://www.bilibili.com/video/BV1QB4y1D7ep/)

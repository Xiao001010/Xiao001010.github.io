---
title:       "代码随想录Leetcode Day4 Linked List Part 2"
subtitle:    ""
description: "24. Swap Nodes in Pairs, 19. Remove Nth Node From End of List, 160. Intersection of Two Linked Lists, 142. Linked List Cycle II"
date:        "2025-01-13T00:03:32Z"
author:      "Hao Xu"
image:       ""
tags:        ["Leetcode", "Job"]
categories:  ["Tech", "Diary"]
draft:       false
---

# [24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)

## Description

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

**Example 1**:

```
Input: head = [1,2,3,4]

Output: [2,1,4,3]
```

Explanation:
![Swap Nodes in Pairs](/img/2025-01-13-Leetcode-Day4-Linked-List2/swap_ex1.jpg)

**Example 2**:

```
Input: head = []

Output: []
```

**Example 3**:

```
Input: head = [1]

Output: [1]
```

**Example 4**:

```
Input: head = [1,2,3]

Output: [2,1,3]
```

**Constraints**:

- The number of nodes in the list is in the range `[0, 100]`.
- `0 <= Node.val <= 100`

## Iteration Solution

![Swap Nodes in Pairs Iteration Solution](/img/2025-01-13-Leetcode-Day4-Linked-List2/24_iter.gif)
[24. 两两交换链表中的节点 | 动画演示 迭代+递归 24. 两两交换链表中的节点 | LeetCode题解](https://leetcode.cn/problems/swap-nodes-in-pairs/solutions/41485/dong-hua-yan-shi-24-liang-liang-jiao-huan-lian-bia)  

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode* dummy_head = new ListNode(0, head);
        ListNode* cur = dummy_head;
        while (cur->next && cur->next->next) {
            ListNode* first = cur->next;
            ListNode* second = cur->next->next;
            cur->next = second;
            first->next = second->next;
            second->next = first;
            cur = first;
        }
        ListNode* result = dummy_head->next;
        delete dummy_head;
        return result;
    }
};
```

由于需要返回头节点，所以需要一个 dummy_head，指向 head 的前一个节点。  
此题如果通过画图，可以很清晰地发现，每次交换两个节点，只需要改变三个指针的指向即可。  

迭代解法的时间复杂度是 O(n)，空间复杂度是 O(1)。  
除了迭代解法，上方GIF的题解来源还给出了递归解法和Stack解法。图中的t指针对应代码中的cur指针，a指针对应first指针，b指针对应second指针。  

# [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

## Description

Given the `head` of a linked list, remove the $n^{th}$ node from the end of the list and return its head.

**Example 1**:  
![Remove Nth Node From End of List](/img/2025-01-13-Leetcode-Day4-Linked-List2/remove_ex1.jpg)

```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```

**Example 2**:

```
Input: head = [1], n = 1
Output: []
```

**Example 3**:

```
Input: head = [1,2], n = 1
Output: [1]
```

**Constraints**:

- The number of nodes in the list is `sz`.
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

**Follow up**: Could you do this in one pass?

## Solution

这题需要删除倒数第 n 个节点，如果是删除第 n 个节点，可以先遍历一遍链表，得到链表长度，然后再遍历一遍链表删除第 n 个节点。  但是这样需要遍历两遍链表，复杂 度是 $O(n)$。  

所以，考虑使用双指针，两个指针之间相差 n-1  个节点，当后面的指针到达链表尾部时，前面的指针正好指向倒数第 n 个节点的前一个节点。  

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy_head = new ListNode(0, head);
        ListNode* slow = dummy_head;
        ListNode* fast = dummy_head;
        for (int i = 0; i <= n; i++) {
            if (fast == nullptr) {
                // 如果 n 大于链表长度，直接返回原链表
                return head;
            }
            fast = fast->next;
        }
        while (fast) {
            fast = fast->next;
            slow = slow->next;
        }
        ListNode* to_delete = slow->next;
        slow->next = to_delete->next;
        ListNode* result = dummy_head->next;
        delete dummy_head;
        delete to_delete;
        return result;
    }
};
```

# [160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

## Description

Given the heads of two singly linked-lists `headA` and `headB`, return the node at which the two lists intersect. If the two linked lists have no intersection at all, return `null`.

For example, the following two linked lists begin to intersect at node `c1`:
![Intersection of Two Linked Lists](/img/2025-01-13-Leetcode-Day4-Linked-List2/160_statement.png)
The test cases are generated such that there are no cycles anywhere in the entire linked structure.

**Note** that the linked lists must **retain their original structure** after the function returns.

**Custom Judge**:

The inputs to the judge are given as follows (your program is not given these inputs):

`intersectVal` - The value of the node where the intersection occurs. This is 0 if there is no intersected node.
`listA` - The first linked list.
`listB` - The second linked list.
`skipA` - The number of nodes to skip ahead in `listA` (starting from the head) to get to the intersected node.
`skipB` - The number of nodes to skip ahead in `listB` (starting from the head) to get to the intersected node.
The judge will then create the linked structure based on these inputs and pass the two heads, `headA` and `headB` to your program. If you correctly return the intersected node, then your solution will be **accepted**.

Example 1:
![Intersection of Two Linked Lists](/img/2025-01-13-Leetcode-Day4-Linked-List2/160_example_1_1.png)

```
Input: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
Output: Intersected at '8'
Explanation: The intersected node's value is 8 (note that this must not be 0 if the two lists intersect).  
From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,6,1,8,4,5].   
There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.

- Note that the intersected node's value is not 1   
because the nodes with value 1 in A and B (2nd node in A and 3rd node in B) are different node references.   
In other words, they point to two different locations in memory,   
while the nodes with value 8 in A and B (3rd node in A and 4th node in B) point to the same location in memory.
```

Example 2:
![Intersection of Two Linked Lists](/img/2025-01-13-Leetcode-Day4-Linked-List2/160_example_2.png)

```
Input: intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
Output: Intersected at '2'
Explanation: The intersected node's value is 2 (note that this must not be 0 if the two lists intersect).
From the head of A, it reads as [1,9,1,2,4]. From the head of B, it reads as [3,2,4].   
There are 3 nodes before the intersected node in A; There are 1 node before the intersected node in B.
```

Example 3:
![Intersection of Two Linked Lists](/img/2025-01-13-Leetcode-Day4-Linked-List2/160_example_3.png)

```
Input: intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
Output: No intersection
Explanation: From the head of A, it reads as [2,6,4]. From the head of B, it reads as [1,5].   
Since the two lists do not intersect, intersectVal must be 0,   
while skipA and skipB can be arbitrary values.
Explanation: The two lists do not intersect, so return null.
```

**Constraints**:

- The number of nodes of `listA` is in the `m`.
- The number of nodes of `listB` is in the `n`.
- `1 <= m, n <= 3 * 104`
- `1 <= Node.val <= 105`
- `0 <= skipA <= m`
- `0 <= skipB <= n`
- `intersectVal` is `0` if `listA` and `listB` do not intersect.
- `intersectVal == listA[skipA] == listB[skipB]` if `listA` and `listB` intersect.

Follow up: Could you write a solution that runs in $O(m + n)$ time and use only $O(1)$ memory?

## Solution

这里需要注意理解Example 1中的Note，即两个链表的交点不是值相同的节点，而是内存地址相同的节点。  
在输入函数前，两个链表相交的节点就已经是同一个节点了，而不是两个值相同的不同节点。  

这题的思路是，当两个链表在末端相交时，末尾的节点是相同的，所以可以先遍历两个链表得到链表长度，然后将较长的链表的指针向前移动，使得两个链表末端对齐。  
这样再比较两个链表的节点，找到相交的节点。  

这个思路的时间复杂度是 $O(m+n)$，空间复杂度是 $O(1)$。  

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        int sizeA = 0, sizeB = 0;
        ListNode* curA = headA;
        ListNode* curB = headB;

        // 对齐指针
        while (curA) {
            sizeA++;
            curA = curA->next;
        }
        while (curB) {
            sizeB++;
            curB = curB->next;
        }

        curA = headA;
        curB = headB;
        if (sizeA < sizeB) {
            for (int i = 0; i < sizeB - sizeA; i++)     curB = curB->next;
        }
        else if (sizeA > sizeB) {
            for (int i = 0; i < sizeA - sizeB; i++)     curA = curA->next;
        }

        // 寻找相交节点
        while (curA != curB) {
            curA = curA->next;
            curB = curB->next;
        }

        ListNode* result = curA == nullptr ? nullptr : curA;
        return result;
    }
};
```

# [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

## Description

Given the `head` of a linked list, return the node where the cycle begins. If there is no cycle, return `null`.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to (**0-indexed**). It is `-1` if there is no cycle. **Note that `pos` is not passed as a parameter**.

**Do not modify** the linked list.

**Example 1**:
![Linked List Cycle II Example 1](/img/2025-01-13-Leetcode-Day4-Linked-List2/142_ex1.png)

```
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```

**Example 2**:
![Linked List Cycle II Example 2](/img/2025-01-13-Leetcode-Day4-Linked-List2/142_ex2.png)

```
Input: head = [1,2], pos = 0
Output: tail connects to node index 0
Explanation: There is a cycle in the linked list, where tail connects to the first node.
```

**Example 3**:
![Linked List Cycle II Example 3](/img/2025-01-13-Leetcode-Day4-Linked-List2/142_ex3.png)

```
Input: head = [1], pos = -1
Output: no cycle
Explanation: There is no cycle in the linked list.
```

**Constraints**:

- The number of the nodes in the list is in the range `[0, 104]`.
- `-105 <= Node.val <= 105`
- `pos` is `-1` or a **valid index** in the linked-list.

**Follow up**: Can you solve it using $O(1)$ (i.e. constant) memory?

## Solution

这题判断链表中是否有环很简单，只需要使用快慢指针，如果快指针追上了慢指针，说明有环，如果快指针变成了 nullptr，说明没有环。  

然而对于这题，需要找到环的起点，这就需要画图并进行一定的数学推导。  

![142. Linked List Cycle II Solution](/img/2025-01-13-Leetcode-Day4-Linked-List2/142_solution.png)
[142. 环形链表 II | 详细图解(肯定看的明白) | LeetCode题解](https://leetcode.cn/problems/linked-list-cycle-ii/solutions/181070/xiang-xi-tu-jie-ken-ding-kan-de-ming-bai-by-xixili)  

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast = head;
        ListNode* slow = head;
        while (fast != nullptr && fast->next != nullptr) {
            fast = fast->next->next;
            slow = slow->next;
            if (fast == slow) {
                slow = head;
                while (slow != fast) {
                    slow = slow->next;
                    fast = fast->next;
                }
                return fast;
            }
        }
        return nullptr;
    }
};
```

# Reference

[代码随想录](https://programmercarl.com/)  
[24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)  
[24. 两两交换链表中的节点 | 动画演示 迭代+递归 24. 两两交换链表中的节点 | LeetCode题解](https://leetcode.cn/problems/swap-nodes-in-pairs/solutions/41485/dong-hua-yan-shi-24-liang-liang-jiao-huan-lian-bia)  
[19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)  
[160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)  
[142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)  
[142. 环形链表 II | 详细图解(肯定看的明白) | LeetCode题解](https://leetcode.cn/problems/linked-list-cycle-ii/solutions/181070/xiang-xi-tu-jie-ken-ding-kan-de-ming-bai-by-xixili)  

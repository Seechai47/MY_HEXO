---
title: 002. Add Two Numbers
copyright: true
related_posts: true
abbrlink: '524851e8'
date: 2020-01-17 21:46:39
tags:
categories:
---

<!-- more -->

# 002. Add Two Numbers
You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example:**
```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

## Solution
**核心思想：**正确处理进位
我们使用一个变量carry（初始化为0）来跟踪进位，遍历l1，l2的节点从低位到高位依次进行加法模拟，每位的计算完成后将carry带入下一次迭代。

**Notes：**carry的值必定为0或1，因为个位数相加（考虑进位）可能出现的最大和为：9+9+1=19。处理max(m, n)位时，要考虑是否有额外进位。

**特殊情况：**

| 测试用例                     | 说明            |
|--------------------------|---------------|
| l1 = [0,1], l2 = [0,1,2] | 当一个列表比另一个列表长时 |
| l1 = [], l2 = [0,1,2]    | 当其中一个列表为空时    |
| l1 = [1], l2 = [9,9,9]   | 求和运算最后可能出现额外的进位，这一点很容易被遗忘       |



## Complexity
**时间复杂度：**O(max(m, n))，假设l1和l2的长度为m和n，对链表的处理最多执行max(m, n)次；
**时间复杂度：**O(max(m, n))，新链表的节点数最多为max(m, n) + 1个。

## Code
**初始版本：**
```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* res = new ListNode(0);
        ListNode* cur = res;

        int carry = 0;
        while (l1 != nullptr && l2 != nullptr) {
            cur->next = new ListNode((l1->val + l2->val + carry) % 10);
            carry = (l1->val + l2->val + carry) / 10;
            cur = cur->next;
            l1 = l1->next; l2 = l2->next;
        }

        while (l1 != nullptr) {
            cur->next = new ListNode((l1->val + carry) % 10);
            carry = (l1->val + carry) / 10;
            cur = cur->next;
            l1 = l1->next;
        }

        while (l2 != nullptr) {
            cur->next = new ListNode((l2->val + carry) % 10);
            carry = (l2->val + carry) / 10;
            cur = cur->next;
            l2 = l2->next;
        }

        if (carry != 0)
            cur->next = new ListNode(carry);

        return res->next;
    }
};
```

**简化版本：**
```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode preHead(0), *p = &preHead;
        int extra = 0;
        while (l1 || l2 || extra) {
            if (l1) extra += l1->val, l1 = l1->next;
            if (l2) extra += l2->val, l2 = l2->next;
            p->next = new ListNode(extra % 10);
            extra /= 10;
            p = p->next;
        }
        return preHead.next;
    }
};
```




---
layout: post
title: leetcode-002-Add Two Numbers
date: 2018-06-10 22:03：01 +08:00
category:
    - leetcode
keywords: C++ leetcode
tags:
    - leetcode
---

# 题目

**EN**:You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**CN**: 给定两个非空链表来表示两个非负整数。位数按照逆序方式存储，它们的每个节点只存储单个数字。将两数相加返回一个新的链表。

你可以假设除了数字 0 之外，这两个数字都不会以零开头。

例如：

> Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
>
> Output: 7 -> 0 -> 8
>
> Explanation: 342 + 465 = 807.

# 实现

```cpp
/*
You are given two non-empty linked lists representing two non-negative integers.
The digits are stored in reverse order and each of their nodes contain a single digit.
Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Example
--------------------------------------------------------------------------------
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
--------------------------------------------------------------------------------
*/

#include <iostream>

/* Definition for singly-linked list.*/
struct ListNode {
	int val;
	ListNode *next;
	ListNode(int x) : val(x), next(NULL) {}
};

class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
		int a	  = 0;
		int b	  = 0;
		int carry = 0;
		int sum   = 0;

		ListNode *h  = NULL;
		ListNode **t = &h;

		while(l1 != NULL || l2 != NULL)
		{
			a = getValAndMoveNext(l1);
			b = getValAndMoveNext(l2);

			sum = carry + a + b;

			ListNode *node = new ListNode(sum % 10);

			*t = node;
			t = (&node->next);

			carry = sum / 10;
		}

		if (carry > 0)
		{
			ListNode *node = new ListNode(carry % 10);
			*t = node;
		}

		return h;
    }

private:
	int getValAndMoveNext(ListNode* &l){
		int x = 0;
		if (l != NULL)
		{
			x = l->val;
			l = l->next;
		}

		return x;
	}
};

ListNode* createList(int arr[], int len)
{
	ListNode *h  = NULL;
	ListNode **t = &h;

	for (int i = 0; i < len; i++)
	{
		ListNode *node = new ListNode(arr[i]);
		*t = node;
		t = (&node->next);
	}

	return h;

}

void printList(ListNode *l)
{
	ListNode* tmp = l;

	while(tmp != NULL)
	{
		std::cout << tmp->val;
		tmp = tmp->next;
		if (tmp != NULL)
		{
			std::cout << "->";
		}
	}
	std::cout << std::endl;
}

int main()
{
	Solution slt;

	ListNode *h = NULL;

	int a[] = {2 ,4, 3};
	int b[] = {5, 6, 4};

	ListNode *l1 = NULL;
	ListNode *l2 = NULL;

	l1 = createList(a, sizeof(a)/sizeof(a[0]));
	l2 = createList(b, sizeof(b)/sizeof(b[0]));

	h = slt.addTwoNumbers(l1, l2);

	std::cout << "l1 list:";
	printList(l1);

	std::cout << "l2 list:";
	printList(l2);

	std::cout << "l1 + l2 result:";
	printList(h);

	delete l1;
	delete l2;
	delete h;
	return 0;
}

```

---
title: LeetCode 141.Linked List Cycle
date: 2017-03-17 22:26:50
tags:
	- 题解
	- LeetCode
---

Description:

Given a linked list, determine if it has a cycle in it.

Follow up:
Can you solve it without using extra space?

<!-- more -->

使用双指针，一个每次走一步，另一个每次走两步，若存在环，指针将出现相等的情况。

代码:

	class Solution {
	public:
	    bool hasCycle(ListNode *head) {
	        ListNode *p1 = head;
	        ListNode *p2 = head;
	        while(p2 && p2->next){
	            p1 = p1->next;
	            p2 = p2->next->next;
	            if(p1 == p2)
	                return true;
	        }
	        return false;
	    }
	};


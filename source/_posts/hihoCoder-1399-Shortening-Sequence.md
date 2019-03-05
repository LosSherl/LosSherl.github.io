---
title: hihoCoder 1399 Shortening Sequence
date: 2017-03-21 22:25:14
tags:
	- 题解
	- hihoCoder
---

时间限制:10000ms
单点时限:1000ms
内存限制:256MB
描述
There is an integer array A1, A2 ...AN. Each round you may choose two adjacent integers. If their sum is an odd number, the two adjacent integers can be deleted.

Can you work out the minimum length of the final array after elaborate deletions?

<!-- more --> 

输入
The first line contains one integer N, indicating the length of the initial array.

The second line contains N integers, indicating A1, A2 ...AN.

For 30% of the data：1 ≤ N ≤ 10

For 60% of the data：1 ≤ N ≤ 1000

For 100% of the data：1 ≤ N ≤ 1000000, 0 ≤ Ai ≤ 1000000000

输出
One line with an integer indicating the minimum length of the final array.

样例提示
(1,2) (3,4) (4,5) are deleted.

样例输入
7
1 1 2 3 4 4 5
样例输出
1

思路：
  动态规划，线性扫描并枚举26个字母。

代码:
	#include <iostream>
	#include <vector>
	#include <algorithm>
	#include <cmath>
	#include <cstdio>
	using namespace std;

	//strucc p{
	//	int cur;
	//	int last;
	//	int len;
	//}dp[1000001];

	int n;

	int main()
	{
		cin >> n;
		vector<int> v;
		int t;
		scanf("%d",&t);
		v.push_back(t);
		for(int i = 2; i <= n; i++)
		{
			scanf("%d",&t);
			if(v.size() == 0)
			{
				v.push_back(t);
				continue;
			}
			if((t ^ v[v.size() - 1]) & 1)
			{
				v.erase(v.end() - 1);
			}
			else
				v.push_back(t);
		}

		cout << v.size();
	}
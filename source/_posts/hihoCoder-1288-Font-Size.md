---
title: hihoCoder 1288 Font Size
date: 2017-03-21 22:21:08
tags:
	- 题解
	- hihoCoder
---

时间限制:10000ms
单点时限:1000ms
内存限制:256MB
描述
Steven loves reading book on his phone. The book he reads now consists of N paragraphs and the i-th paragraph contains ai characters.

Steven wants to make the characters easier to read, so he decides to increase the font size of characters. But the size of Steven's phone screen is limited. Its width is W and height is H. As a result, if the font size of characters is S then it can only show ⌊W / S⌋ characters in a line and ⌊H / S⌋ lines in a page. (⌊x⌋ is the largest integer no more than x)  

So here's the question, if Steven wants to control the number of pages no more than P, what's the maximum font size he can set? Note that paragraphs must start in a new line and there is no empty line between paragraphs.

<!-- more -->
输入
Input may contain multiple test cases.

The first line is an integer TASKS, representing the number of test cases.

For each test case, the first line contains four integers N, P, W and H, as described above.

The second line contains N integers a1, a2, ... aN, indicating the number of characters in each paragraph.



For all test cases,

1 <= N <= 103,

1 <= W, H, ai <= 103,

1 <= P <= 106,

There is always a way to control the number of pages no more than P.

输出
For each testcase, output a line with an integer Ans, indicating the maximum font size Steven can set.

样例输入
2
1 10 4 3
10
2 10 4 3
10 10
样例输出
3
2


思路：
  二分搜索。

代码：


	#include <iostream>
	#include <vector>
	#include <algorithm>
	#include <cmath>
	#include <cstdio>
	#include <cstring>
	#include <string>
	#include <queue>
	using namespace std;

	int task;
	int w,h,p;
	int n;
	int a[1001] = {0};

	int main()
	{
		cin >> task;	
		for(int z = 1; z <= task; z++)
		{
			cin >> n >> p >> w >> h;
			for(int i = 1; i <= n; i++)
				cin >> a[i];
			int l = 1;
			int r = min(w,h);
			int mid;
			int last;
			while(l <= r)
			{
				mid = (l + r) / 2;
				int line = h / mid;
				int wid = w / mid;
				int count = 0;
				for(int j = 1; j <= n; j++)
				{
					count += a[j] / wid;
					if(a[j] % wid)
						count++;
				}
				if(count <= p * line)
				{
					last = mid;
					l = mid + 1;
				}
				else
				{
					r = mid - 1;
				}
			}
			cout << last << endl;
		}
	}
---
title: hihoCoder 1238 Total Highway Distance
date: 2017-03-21 22:28:30
tags:
	- 题解
	- hihoCoder
---

时间限制:10000ms
单点时限:1000ms
内存限制:256MB
描述
Little Hi and Little Ho are playing a construction simulation game. They build N cities (numbered from 1 to N) in the game and connect them by N-1 highways. It is guaranteed that each pair of cities are connected by the highways directly or indirectly.

The game has a very important value called Total Highway Distance (THD) which is the total distances of all pairs of cities. Suppose there are 3 cities and 2 highways. The highway between City 1 and City 2 is 200 miles and the highway between City 2 and City 3 is 300 miles. So the THD is 1000(200 + 500 + 300) miles because the distances between City 1 and City 2, City 1 and City 3, City 2 and City 3 are 200 miles, 500 miles and 300 miles respectively.

During the game Little Hi and Little Ho may change the length of some highways. They want to know the latest THD. Can you help them?

<!-- more -->

输入
Line 1: two integers N and M.

Line 2 .. N: three integers u, v, k indicating there is a highway of k miles between city u and city v.

Line N+1 .. N+M: each line describes an operation, either changing the length of a highway or querying the current THD. It is in one of the following format.

EDIT i j k, indicating change the length of the highway between city i and city j to k miles.

QUERY, for querying the THD.

For 30% of the data: 2<=N<=100, 1<=M<=20

For 60% of the data: 2<=N<=2000, 1<=M<=20

For 100% of the data: 2<=N<=100,000, 1<=M<=50,000, 1 <= u, v <= N, 0 <= k <= 1000.

输出
For each QUERY operation output one line containing the corresponding THD.

样例输入
3 5
1 2 2
2 3 3
QUERY
EDIT 1 2 4
QUERY
EDIT 2 3 2
QUERY
样例输出
10
14
12

思路：
	两次深搜计算每条路径经过的次数。

代码：

	#include <iostream>
	#include <vector>
	#include <queue>
	#include <cmath>
	#include <algorithm>
	#include <cstring> 
	#include <string>
	#include <cstdio>
	using namespace std;

	typedef struct p{
		int cur;
		vector<int> v;
		p(int c)
		{
			cur = c;
		}
	}p;

	typedef struct ar{
		int to;
		int index;
		ar(int t,int i)
		{
			to = t;
			index = i;
		}
	}ar;

	typedef struct arcc{
		int f;
		int t;
		int v;
	}arcc;

	vector<ar> a[100010];

	bool vi[100010] = {0};
	arcc arc[100010];
	long long arc_c[100010] = {0};
	int n,m;
	int child[100010] = {0};

	long long query()
	{
		long long ans = 0;
		for(int i = 1; i < n; i++)
		{
			ans += arc[i].v * arc_c[i];
		}
		return ans;
	}

	void dfs(int x,int f)
	{
		child[x] = 1;
		for(int i = 0; i < a[x].size(); i++)
		{
			if(a[x][i].to == f)
				continue;
			dfs(a[x][i].to,x);
			child[x] += child[a[x][i].to];
		}
	}

	void dfs1(int x,int f)
	{
		for(int i = 0; i < a[x].size(); i++)
		{
			if(a[x][i].to == f)
				continue;
			arc_c[a[x][i].index] = (long long) child[a[x][i].to] * (long long)(n - child[a[x][i].to]); 
			dfs1(a[x][i].to,x);
		}
	}

	int main()
	{
		cin >> n >> m;
		
		for(int i = 1; i < n; i++)
		{
			int f,t,v;
			scanf("%d %d %d",&f,&t,&v);
			arc[i].v = v;
			arc[i].f = f;
			arc[i].t = t;
			a[f].push_back(ar(t,i));
			a[t].push_back(ar(f,i));
		}
		
		dfs(1,0);
		dfs1(1,0);
		
	//	for(int i = 1; i < n; i++)
	//		cout << arc_c[i] << endl;
		
		string cmd;
		long long ans = query();
		for(int i = 1; i <= m; i++)
		{
			cin >> cmd;
			if(cmd == "QUERY")
			{
				printf("%lld\n",ans);
			}
			else
			{
				int f,t,v;
				scanf("%d %d %d",&f,&t,&v);
				for(int j = 0; j < a[f].size(); j++)
				{
					if(a[f][j].to == t)
					{
						ans += arc_c[a[f][j].index] * (v - arc[a[f][j].index].v);
						arc[a[f][j].index].v = v;
						break;
					}
				}
			}
		}
	}
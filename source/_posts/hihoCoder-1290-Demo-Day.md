---
title: hihoCoder 1290 Demo Day
date: 2017-03-23 00:25:25
tags:
	- 题解
	- hihoCoder
---

描述
You work as an intern at a robotics startup. Today is your company's demo day. During the demo your company's robot will be put in a maze and without any information about the maze, it should be able to find a way out.

The maze consists of N * M grids. Each grid is either empty(represented by '.') or blocked by an obstacle(represented by 'b'). The robot will be release at the top left corner and the exit is at the bottom right corner.

Unfortunately some sensors on the robot go crazy just before the demo starts. As a result, the robot can only repeats two operations alternatively: keep moving to the right until it can't and keep moving to the bottom until it can't. At the beginning, the robot keeps moving to the right.

rrrrbb..            
...r....     ====> The robot route with broken sensors is marked by 'r'. 
...rrb..
...bb...
While the FTEs(full-time employees) are busy working on the sensors, you try to save the demo day by rearranging the maze in such a way that even with the broken sensors the robot can reach the exit successfully. You can change a grid from empty to blocked and vice versa. So as not to arouse suspision, you want to change as few grids as possible. What is the mininum number?

<!-- more -->

输入
Line 1: N, M.

Line 2-N+1: the N * M maze.



For 20% of the data, N * M <= 16.

For 50% of the data, 1 <= N, M <= 8.

For 100% of the data, 1<= N, M <= 100.

输出
The minimum number of grids to be changed.

样例输入
4 8
....bb..
........
.....b..
...bb...
样例输出
1

思路：
  动态规划，dp[i][j][k]表示到达(i,j)点以k方向前进最小所需做改变的次数。


代码:
	#include <iostream>
	using namespace std;

	int m,n;
	bool map[110][110] = {0};
	int dp[110][110][2] = {0};

	int main()
	{
		cin >> n >> m;
		
		for(int i = 1; i <= n; i++)
		{
			for(int j = 1; j <= m; j++)
			{
				char c;
				cin >> c;
				if(c == '.')
				{
					map[i][j] = true;
				}
			}
		}
		
		for(int i = 0; i <= n; i++)
		{
			for(int j = 0; j <= m; j++)
			{
				dp[i][j][0] = dp[i][j][1] = 999999; 
			}
		}
		dp[1][1][0] = 0;
		for(int i = 1; i <= n; i++)
		{
			for(int j = 1; j <= m; j++)
			{
				for(int k = 0; k < 2; k++)
				{
					if(map[i][j])
					{
						if(k)
						{
							dp[i][j][k] = min(dp[i][j][k],dp[i - 1][j][k]);
							if(!map[i + 1][j])
							{
								dp[i][j][1 - k] = min(dp[i][j][1 - k],dp[i][j][k]);
							}
							else
							{
								dp[i][j][1 - k] = min(dp[i][j][1 - k],dp[i][j][k] + 1);
							}
						}
						else
						{
							dp[i][j][k] = min(dp[i][j][k],dp[i][j - 1][k]);
							if(!map[i][j + 1])
							{
								dp[i][j][1 - k] = min(dp[i][j][1 - k],dp[i][j][k]);
							}
							else
							{
								dp[i][j][1 - k] = min(dp[i][j][1 - k],dp[i][j][k] + 1);
							}
						}
					}
					else
					{
						if(k)
						{
							dp[i][j][k] = min(dp[i][j][k],dp[i - 1][j][k] + 1);
							if(!map[i + 1][j])
							{
								dp[i][j][1 - k] = min(dp[i][j][1 - k],dp[i][j][k]);
							}
							else
							{
								dp[i][j][1 - k] = min(dp[i][j][1 - k],dp[i][j][k] + 1);
							}
						}
						else
						{
							dp[i][j][k] = min(dp[i][j][k],dp[i][j - 1][k] + 1);
							if(!map[i][j + 1])
							{
								dp[i][j][1 - k] = min(dp[i][j][1 - k],dp[i][j][k]);
							}
							else
							{
								dp[i][j][1 - k] = min(dp[i][j][1 - k],dp[i][j][k] + 1);
							}
						}
					}
	//				cout << i << " " << j << " " << k << " " << dp[i][j][k] << endl;
				}
			}
		}
		cout << min(dp[n][m][0],dp[n][m][1]) << endl;
		
		
	}

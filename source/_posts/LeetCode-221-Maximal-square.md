---
title: LeetCode 221.Maximal square
date: 2017-03-17 22:17:50
tags:
	- 题解
	- LeetCode
---

Description:

Given a 2D binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.

For example, given the following matrix:

1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0

Return 4.

<!-- more -->

动态规划可解

代码:

	class Solution {
	public:
	    int maximalSquare(vector<vector<char>>& matrix) {
	        int n = matrix.size();
	        if(n == 0)
	            return 0;
	        int m = matrix[0].size();
	        if(m == 0)
	            return 0;
	        int ans = 0;
	        vector<vector<int>> dp(n + 1);
	        for(int i = 0; i < dp.size(); i++)
	        {
	            dp[i].resize(m + 1);
	        }
	        for(int i = 1; i <= n; i++)
	        {
	            for(int j = 1; j <= m; j++)
	            {
	                if(matrix[i - 1][j - 1] == '1')
	                {
	                    dp[i][j] = min(min(dp[i-1][j],dp[i][j-1]),dp[i-1][j-1]) + 1;
	                }
	                ans = max(ans,dp[i][j]);
	            }
	        }
	        return ans * ans;
	        
	    }
	   
	};


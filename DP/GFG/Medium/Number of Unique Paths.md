### Code:
```cpp
class Solution{
    public:
    int solveMem(int i, int j, int n, int m, vector<vector<int>>& dp) {
        if (i == (n - 1) && j == (m - 1)) return 1; 
        if (i >= n || j >= m) return 0;           
        if (dp[i][j] != -1) return dp[i][j];        
        dp[i][j] = solveMem(i + 1, j, n, m, dp) + solveMem(i, j + 1, n, m, dp);
        return dp[i][j];
    }

    int NumberOfPath(int a, int b) {
        vector<vector<int>> dp(a, vector<int>(b, -1));
        return solveMem(0, 0, a, b, dp);              
    }
};
```
```cpp
class Solution {
  public:
    // Recursive function (solveRec)
    int solveRec(string &str1, string &str2, int i, int j) {
        if (i == str1.size() || j == str2.size()) return 0;
        
        if (str1[i] == str2[j])
            return 1 + solveRec(str1, str2, i + 1, j + 1);
        else
            return max(solveRec(str1, str2, i + 1, j), solveRec(str1, str2, i, j + 1));
    }
    
    // Memoization function (solveMem)
    int solveMem(string &str1, string &str2, int i, int j, vector<vector<int>> &memo) {
        if (i == str1.size() || j == str2.size()) return 0;
        if (memo[i][j] != -1) return memo[i][j];
        
        if (str1[i] == str2[j])
            memo[i][j] = 1 + solveMem(str1, str2, i + 1, j + 1, memo);
        else
            memo[i][j] = max(solveMem(str1, str2, i + 1, j, memo), solveMem(str1, str2, i, j + 1, memo));
        
        return memo[i][j];
    }
    
    // Tabulation function (solveTab)
    int solveTab(string &str1, string &str2) {
        int m = str1.size(), n = str2.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        
        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                if (str1[i] == str2[j])
                    dp[i][j] = 1 + dp[i + 1][j + 1];
                else
                    dp[i][j] = max(dp[i + 1][j], dp[i][j + 1]);
            }
        }
        
        return dp[0][0];
    }
    
    // Space-Optimized function (solveSO)
    int solveSO(string &str1, string &str2) {
        int m = str1.size(), n = str2.size();
        vector<int> curr(n + 1, 0), next(n + 1, 0);
        
        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                if (str1[i] == str2[j])
                    curr[j] = 1 + next[j + 1];
                else
                    curr[j] = max(next[j], curr[j + 1]);
            }
            swap(curr, next);
        }
        
        return next[0];
    }
    
    // Function to find the length of longest common subsequence in two strings.
    int lcs(int n, int m, string str1, string str2) {
        // Recursive call
        // return solveRec(str1, str2, 0, 0);

        // Memoization call
        // vector<vector<int>> memo(n, vector<int>(m, -1));
        // return solveMem(str1, str2, 0, 0, memo);

        // Tabulation call
        return solveTab(str1, str2);

        // Space-Optimized call
        // return solveSO(str1, str2);
    }
};

```

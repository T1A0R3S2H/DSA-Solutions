```cpp
class Solution{
  public:
  
    // Recursive function (solveRec)
    int solveRec(string &text1, string &text2, int i, int j) {
        if (i==text1.size() || j==text2.size()) return 0;
        if (text1[i]==text2[j]){
            return 1+solveRec(text1, text2, i+1, j+1);
        }
        else{
            return max(solveRec(text1, text2, i+1, j), solveRec(text1, text2, i, j+1));
        }
    }
    
    // Memoization function (solveMem)
    int solveMem(string &text1, string &text2, int i, int j, vector<vector<int>> &memo) {
        if (i==text1.size() || j==text2.size()) return 0;
        if (memo[i][j]!=-1) return memo[i][j];
        if (text1[i]==text2[j]){
            memo[i][j]=1+solveMem(text1, text2, i+1, j+1, memo);
        }
        else{
            memo[i][j] = max(solveMem(text1, text2, i + 1, j, memo), solveMem(text1, text2, i, j + 1, memo));
        }
        return memo[i][j];
    }
    
    // Tabulation function (solveTab)
    int solveTab(string &text1, string &text2) {
        int m=text1.size(), n = text2.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        for (int i= m-1; i>=0; i--) {
            for (int j=n-1; j >= 0; j--) {
                if (text1[i]==text2[j]) dp[i][j]=1+dp[i+1][j+1];
                else dp[i][j]=max(dp[i+1][j], dp[i][j+1]);
            }
        }
        return dp[0][0];
    }
    
    // Space-Optimized function (solveSO)
    int solveSO(string &text1, string &text2) {
        int m=text1.size(), n = text2.size();
        vector<int> curr(n+1, 0), next(n + 1, 0);
        for (int i=m-1; i>=0; i--) {
            for (int j=n-1; j>=0; j--) {
                if (text1[i]==text2[j]) curr[j]=1+next[j+1];
                else curr[j]=max(next[j], curr[j + 1]);
            }
            swap(curr, next);
        }
        return next[0];
    }
    
    
    int longestPalinSubseq(string A) {
        string revA=A;
        reverse(revA.begin(), revA.end());
        int ans=solveTab(A, revA);
        return ans;
    }
};
```

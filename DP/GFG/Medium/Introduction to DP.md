```cpp
class Solution {
  public:
    int solve(int n, std::vector<int>& dp) {
        if (n == 0) return 0;
        if (n == 1) return 1;
        if (dp[n] != -1) return dp[n];
        dp[n] = (solve(n - 1, dp) + solve(n - 2, dp)) % 1000000007;
        return dp[n];
    }
        
        
    long long int topDown(int n) {
            // code here
            vector<int>dp(n+1,-1);
            return solve(n,dp);
    }
    
    long long int bottomUp(int n) {
        // code here
        if(n == 0) return 0;
        if(n == 1) return 1;
        vector<int>dp(n + 1);
        dp[0] = 0;
        dp[1] = 1;
        for(int i=2;i<= n;i++) {
            dp[i] = (dp[i-1] + dp[i-2]) % 1000000007;
        } 
        return dp[n];
    }
};
```

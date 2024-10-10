# Method 1 (Tabulation, Iterative)
```cpp
class Solution {
public:
    // iterative, tabulation approach
    // test cases are small, so memoisation will better when time is considered
    int climbStairs(int n) {
        if(n==0||n==1) return 1;
        vector<int>dp(n+1);
        dp[0]=1;
        dp[1]=1;
        for(int i=2;i<=n;i++){
            dp[i]=dp[i-1]+dp[i-2];
        }
        return dp[n];
    }
};
```
# Method 2 (Memoisation, Recursive)  
```cpp
class Solution {
public:
    // Recursive memoization approach
    // less number of test cases, there memoisation will be time optimal
    int solve(int n, vector<int>& dp) {
        if(n==0 || n==1) return 1;
        if(dp[n]!=-1) return dp[n];
        dp[n]=solve(n-1, dp)+solve(n-2, dp);
        return dp[n];
    }

    int climbStairs(int n) {
        vector<int> dp(n+1, -1);
        return solve(n, dp);
    }
};

```

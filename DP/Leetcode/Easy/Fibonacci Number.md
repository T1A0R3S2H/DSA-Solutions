```cpp
class Solution {
public:
    int fn(int n, vector<int> &dp) {
        if (n<=1) return n;
        if (dp[n]!=-1) return dp[n];
        dp[n]=fn(n-1, dp)+fn(n-2, dp);
        return dp[n];
    }
    int fib(int n) {
        vector<int> dp(n+1, -1);  // Initialize dp array with -1
        return fn(n, dp);
    }
};
```

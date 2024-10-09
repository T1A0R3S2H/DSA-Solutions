# 🔥Method 1 (Top-down approach) - Recursive - Memoization
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

# 🔥Method 2 (Bottom-up approach) - Iterative - Tabulation
```cpp
class Solution {
public:
    int fn(int n, vector<int> &dp) {
        dp[0]=0;
        if (n > 0) dp[1] = 1;
        for(int i=2; i<=n; i++){
            dp[i]=dp[i-1]+dp[i-2];
        }
        return dp[n];
    }
    int fib(int n) {
        if (n==0) return 0;
        vector<int> dp(n+1, -1);  // Initialize dp array with -1
        return fn(n, dp);
    }
};
```


![image](https://github.com/user-attachments/assets/90bd6362-bbc6-4696-80ec-b7d6ae0e426e)




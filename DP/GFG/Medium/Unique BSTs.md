```cpp
const int MOD = 1e9 + 7;

int solveRec(int n) {
    if (n <= 1) return 1;
    int ans = 0;
    for (int i = 1; i <= n; i++) {
        ans = (ans + (1LL * solveRec(i - 1) * solveRec(n - i)) % MOD) % MOD;
    }
    return ans;
}

int solveMem(int n, vector<int>& dp) {
    if (n <= 1) return 1;
    if (dp[n] != -1) return dp[n];
    int ans = 0;
    for (int i = 1; i <= n; i++) {
        ans = (ans + (1LL * solveMem(i - 1, dp) * solveMem(n - i, dp)) % MOD) % MOD;
    }
    return dp[n] = ans;
}

int solveTab(int n) {
    vector<int> dp(n + 1, 0);
    dp[0] = 1;
    dp[1] = 1;
    for (int nodes = 2; nodes <= n; nodes++) {
        int ans = 0;
        for (int i = 1; i <= nodes; i++) {
            ans = (ans + (1LL * dp[i - 1] * dp[nodes - i]) % MOD) % MOD;
        }
        dp[nodes] = ans;
    }
    return dp[n];
}

// Main function that can be called with an integer N
int numTrees(int N) {
    vector<int> dp(N + 1, -1);
    // Choose between the methods as needed:
    // return solveRec(N);
    // return solveMem(N, dp);
    return solveTab(N);
}

```

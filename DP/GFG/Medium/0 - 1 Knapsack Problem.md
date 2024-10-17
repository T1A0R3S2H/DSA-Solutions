**C++ Code:**

```cpp
class Solution {
  public:
    // Recursive solution
    int recur(int index, int W, vector<int>& wt, vector<int>& val) {
        if (index == 0) {
            if (wt[0] <= W) return val[0];
            return 0;
        }
        int include = 0;
        if (wt[index] <= W) {
            include = val[index] + recur(index - 1, W - wt[index], wt, val);
        }
        int exclude = recur(index - 1, W, wt, val);
        return max(include, exclude);
    }

    // Recursion + Memoization solution
    int solveMem(int index, int W, vector<int>& wt, vector<int>& val, vector<vector<int>>& dp) {
        if (index == 0) {
            if (wt[0] <= W) return val[0];
            return 0;
        }
        if (dp[index][W] != -1) return dp[index][W];
        
        int include = 0;
        if (wt[index] <= W) {
            include = val[index] + solveMem(index - 1, W - wt[index], wt, val, dp);
        }
        int exclude = solveMem(index - 1, W, wt, val, dp);
        return dp[index][W] = max(include, exclude);
    }

    // Tabulation solution
    int solveTab(int W, vector<int>& wt, vector<int>& val) {
        int n = val.size();
        vector<vector<int>> dp(n, vector<int>(W + 1, 0));
        
        for (int w = wt[0]; w <= W; w++) {
            if (wt[0] <= W) dp[0][w] = val[0];
        }

        for (int index = 1; index < n; index++) {
            for (int w = 0; w <= W; w++) {
                int include = 0;
                if (wt[index] <= w) {
                    include = val[index] + dp[index - 1][w - wt[index]];
                }
                int exclude = dp[index - 1][w];
                dp[index][w] = max(include, exclude);
            }
        }

        return dp[n - 1][W];
    }

    // Space-optimized solution
    int solveSO(int W, vector<int>& wt, vector<int>& val) {
        int n = val.size();
        vector<int> prev(W + 1, 0), curr(W + 1, 0);

        for (int w = wt[0]; w <= W; w++) {
            if (wt[0] <= W) prev[w] = val[0];
        }

        for (int index = 1; index < n; index++) {
            for (int w = 0; w <= W; w++) {
                int include = 0;
                if (wt[index] <= w) {
                    include = val[index] + prev[w - wt[index]];
                }
                int exclude = prev[w];
                curr[w] = max(include, exclude);
            }
            prev = curr;
        }

        return prev[W];
    }

    int knapSack(int W, vector<int>& wt, vector<int>& val) {
        int n = val.size();
        // return recur(n - 1, W, wt, val);
        // vector<vector<int>> dp(n, vector<int>(W + 1, -1)); return solveMem(n - 1, W, wt, val, dp);
        // return solveTab(W, wt, val);
        return solveSO(W, wt, val);
    }
};
```

**Explanation:**
1. **recur**: Implements the recursive solution.
2. **solveMem**: Implements recursion with memoization.
3. **solveTab**: Implements the dynamic programming tabulation approach.
4. **solveSO**: Implements the space-optimized dynamic programming solution.

**Time Complexity:**  
All approaches have a time complexity of **O(N * W)**, where `N` is the number of items, and `W` is the capacity of the knapsack.

**Space Complexity:**  
- **Recursive**: O(N) (stack space).
- **Memoization**: O(N * W) (for `dp` table).
- **Tabulation**: O(N * W) (for `dp` table).
- **Space-optimized**: O(W) (for two 1D arrays `prev` and `curr`).

**Dry Run:**
For `W = 4`, `val[] = {1, 2, 3}`, `wt[] = {4, 5, 1}`, the space-optimized solution selects the third item for a value of 3.

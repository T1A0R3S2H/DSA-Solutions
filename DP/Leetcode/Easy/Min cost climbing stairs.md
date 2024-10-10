# Method 1 (T.L.E.)
```cpp
class Solution {
public:
    int solve(vector<int>& cost, int n){
        if(n==0) return cost[0];
        if(n==1) return cost[1];
        int ans=cost[n]+min(solve(cost, n-1), solve(cost, n-2));
        return ans;
    }
    int minCostClimbingStairs(vector<int>& cost) {
        int n=cost.size();
        int ans=min(solve(cost, n-1), solve(cost, n-2));
        return ans;
    }
};
```


The code is facing a **TLE (Time Limit Exceeded)** error because it uses a recursive approach without memoization, leading to repeated recalculations of the same subproblems. To optimize it, you can implement **memoization** or use an **iterative dynamic programming** approach. Here's an optimized solution using dynamic programming:
# Method 2 (DP, Tabulation, Iterative)
### Optimized Approach (Bottom-Up DP):
Instead of solving subproblems recursively, calculate the result iteratively by building up from the base cases.

```cpp
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int n = cost.size();
        // Edge case: if the size is 2 or less
        if (n == 0) return 0;
        if (n == 1) return cost[0];

        // Create a DP array to store minimum costs to reach each step
        vector<int> dp(n);
        
        // Base cases
        dp[0] = cost[0];
        dp[1] = cost[1];
        
        // Fill the dp array for each step from 2 to n-1
        for (int i = 2; i < n; ++i) {
            dp[i] = cost[i] + min(dp[i-1], dp[i-2]);
        }
        
        // The answer is the minimum cost to reach either of the last two steps
        return min(dp[n-1], dp[n-2]);
    }
};
```

- **Explanation**:  
  We calculate the minimum cost to reach each step iteratively, starting from the base cases. The cost to reach step `i` is the cost of the step itself plus the minimum of the costs to reach steps `i-1` and `i-2`. Finally, the result is the minimum cost of reaching either of the last two steps (`n-1` or `n-2`).

- **Time Complexity**:  
  O(n), where `n` is the number of steps. Each step is processed once in the loop.

- **Space Complexity**:  
  O(n) for the DP array. However, this can be further optimized to O(1) by storing only the last two states.

- **Dry Run**:
  - For `cost = [10, 15, 20]`, 
    - `dp[0] = 10`, `dp[1] = 15`.
    - `dp[2] = 20 + min(dp[1], dp[0]) = 20 + min(15, 10) = 30`.
    - The answer is `min(dp[2], dp[1]) = min(30, 15) = 15`.

This approach avoids redundant calculations and resolves the TLE.

# Method 3 (Recursion+Memoization, Top-down)
Define a recursive function that computes the minimum cost to climb to a particular step and memoize the results to avoid redundant calculations.

Here’s how we can implement the top-down approach with memoization:

### Memoization + Recursion Approach (Top-Down)

```cpp
class Solution {
public:
    int solve(int i, vector<int>& cost, vector<int>& dp) {
        // Base cases
        if (i<0) return 0;
        if (i==0 || i==1) return cost[i];
        if (dp[i]!=-1) return dp[i];
        dp[i]=cost[i]+min(solve(i-1, cost, dp), solve(i-2, cost, dp));
        return dp[i];
    }

    int minCostClimbingStairs(vector<int>& cost) {
        int n=cost.size();
        if (n==0) return 0;
        vector<int> dp(n, -1);
        return min(solve(n-1, cost, dp), solve(n-2, cost, dp));
    }
};

```

### Explanation:
1. **Recursion + Memoization**:
    - We use a helper function `minCost(i, cost, memo)` that recursively calculates the minimum cost to reach the `i`-th step.
    - The recursion uses the relation: 
      \[
      \text{minCost}(i) = \text{cost}[i] + \min(\text{minCost}(i-1), \text{minCost}(i-2))
      \]
    - To optimize, we use a memoization array (`memo`) that stores the minimum cost for each step, avoiding recomputation.
  
2. **Base Cases**:
    - If `i == 0` or `i == 1`, the minimum cost is simply `cost[i]`.
    - If `i < 0`, return `0` as it doesn't contribute to the cost.

3. **Final Result**:
    - The minimum cost to reach the top is the minimum of the last two steps: `min(minCost(n-1), minCost(n-2))`.

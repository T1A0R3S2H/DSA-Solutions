
### 1. **solveRec**

#### Code:
```cpp
int solveRec(int index, int buy, vector<int>& prices, int limit) {
    if (index == prices.size()) return 0;
    if (limit == 0) return 0;
    
    if (buy) {
        int buyProfit = -prices[index] + solveRec(index + 1, 0, prices, limit);
        int skipProfit = 0 + solveRec(index + 1, 1, prices, limit);
        return max(buyProfit, skipProfit);
    } else {
        int sellProfit = prices[index] + solveRec(index + 1, 1, prices, limit - 1);
        int skipProfit = solveRec(index + 1, 0, prices, limit);
        return max(sellProfit, skipProfit);
    }
}
```

#### Explanation:
- This recursive function aims to calculate the maximum profit using brute-force recursion.
- Parameters:
  - `index`: The current day (position in the `prices` array).
  - `buy`: Indicates whether we’re allowed to buy (`1` means buying allowed, `0` means selling allowed).
  - `limit`: The remaining number of allowed transactions.
- Base Cases:
  - If `index` reaches the end of the array, return `0` as no further transactions are possible.
  - If `limit` is `0`, return `0` because no more transactions are allowed.
- Recursive Cases:
  - **If `buy` is `1`**: We have two choices:
    - Buy the stock (`-prices[index] + solveRec(index + 1, 0, prices, limit)`).
    - Skip buying (`solveRec(index + 1, 1, prices, limit)`).
    - We return the maximum of these two choices.
  - **If `buy` is `0`**: We have two choices:
    - Sell the stock (`prices[index] + solveRec(index + 1, 1, prices, limit - 1)`).
    - Skip selling (`solveRec(index + 1, 0, prices, limit)`).
    - We return the maximum of these two choices.

#### Time Complexity:
- **O(2^n)**, where `n` is the length of `prices`, due to the exponential growth of recursive calls.

#### Space Complexity:
- **O(n)** due to the recursion stack.

#### Dry Run:
For `prices = [3,3,5,0,0,3,1,4]`, the recursion tree will explore all possible buy/sell decisions at each step, calculating the best profit by combining all possible outcomes for at most two transactions.

---

### 2. **solveMem**

#### Code:
```cpp
int solveMem(int index, int buy, vector<int>& prices, int limit, vector<vector<vector<int>>>& dp) {
    if (index == prices.size() || limit == 0) return 0;
    if (dp[index][buy][limit] != -1) return dp[index][buy][limit];
    
    if (buy) {
        int buyProfit = -prices[index] + solveMem(index + 1, 0, prices, limit, dp);
        int skipProfit = solveMem(index + 1, 1, prices, limit, dp);
        return dp[index][buy][limit] = max(buyProfit, skipProfit);
    } else {
        int sellProfit = prices[index] + solveMem(index + 1, 1, prices, limit - 1, dp);
        int skipProfit = solveMem(index + 1, 0, prices, limit, dp);
        return dp[index][buy][limit] = max(sellProfit, skipProfit);
    }
}
```

#### Explanation:
- This function is a memoized version of `solveRec` to optimize the redundant recursive calls by storing results in a `dp` table.
- `dp[index][buy][limit]` stores the maximum profit at each state, preventing repeated calculations for the same state.
- It uses the same logic as `solveRec` but checks if a result is already computed in `dp` before making recursive calls.

#### Time Complexity:
- **O(n * 2 * 3)**, simplified to **O(n)**, because the three parameters have limited states:
  - `index` goes up to `n`.
  - `buy` has 2 states (0 or 1).
  - `limit` has 3 possible values (0, 1, or 2).

#### Space Complexity:
- **O(n * 2 * 3)** due to the DP table.

#### Dry Run:
For `prices = [3,3,5,0,0,3,1,4]`, `solveMem` will store intermediate results in `dp`, reducing redundant calculations. For example, if `solveMem(3, 1, 2, dp)` is called multiple times, it will reuse the stored result from the first call.

---

### 3. **solveTab**

#### Code:
```cpp
int solveTab(vector<int>& prices) {
    int n = prices.size();
    vector<vector<vector<int>>> dp(n + 1, vector<vector<int>>(2, vector<int>(3, 0)));
    
    for (int index = n - 1; index >= 0; --index) {
        for (int buy = 0; buy <= 1; ++buy) {
            for (int limit = 1; limit <= 2; ++limit) {
                if (buy) {
                    dp[index][buy][limit] = max(-prices[index] + dp[index + 1][0][limit], dp[index + 1][1][limit]);
                } else {
                    dp[index][buy][limit] = max(prices[index] + dp[index + 1][1][limit - 1], dp[index + 1][0][limit]);
                }
            }
        }
    }
    return dp[0][1][2];
}
```

#### Explanation:
- This is a bottom-up DP solution. The table `dp[index][buy][limit]` is filled iteratively, representing the maximum profit from `index` to the end of `prices` for each possible `buy` and `limit` state.
- By filling from the last day to the first, the solution avoids recursion entirely and calculates the profit iteratively.

#### Time Complexity:
- **O(n)** due to the nested loops iterating over `index`, `buy`, and `limit`.

#### Space Complexity:
- **O(n)** for the DP table.

#### Dry Run:
For `prices = [3,3,5,0,0,3,1,4]`, `solveTab` will iteratively fill `dp` from the end to the beginning. For example, `dp[7][1][2]` will depend on `dp[8][0][2]` and `dp[8][1][2]`.

---

### 4. **solveSO**

#### Code:
```cpp
int solveSO(vector<int>& prices) {
    int n = prices.size();
    vector<vector<int>> next(2, vector<int>(3, 0)), curr(2, vector<int>(3, 0));
    
    for (int index = n - 1; index >= 0; --index) {
        for (int buy = 0; buy <= 1; ++buy) {
            for (int limit = 1; limit <= 2; ++limit) {
                if (buy) {
                    curr[buy][limit] = max(-prices[index] + next[0][limit], next[1][limit]);
                } else {
                    curr[buy][limit] = max(prices[index] + next[1][limit - 1], next[0][limit]);
                }
            }
        }
        next = curr;
    }
    return next[1][2];
}
```

#### Explanation:
- Space optimization of `solveTab`. Instead of a 3D DP table, we use two 2D arrays (`curr` and `next`) to store current and next day's values, reducing space complexity.
- `curr` and `next` are alternated to store values as the `index` iterates backward, using only two rows.

#### Time Complexity:
- **O(n)** due to the nested loops.

#### Space Complexity:
- **O(1)** for constant extra space with two 2D arrays of size `2 x 3`.

#### Dry Run:
For `prices = [3,3,5,0,0,3,1,4]`, `solveSO` will iteratively update `curr` and `next`. For example, `curr[1][2]` on day `index` is computed using values from `next`.

---

### 5. **solveMera**

#### Code:
```cpp
int solveMera(vector<int>& prices) {
    int t1Cost = INT_MAX, t2Cost = INT_MAX;
    int t1Profit = 0, t2Profit = 0;

    for (int price : prices) {
        t1Cost = min(t1Cost, price);                    // Minimum price for the 1st transaction
        t1Profit = max(t1Profit, price - t1Cost);       // Max profit for the 1st transaction
        t2Cost = min(t2Cost, price - t1Profit);         // Minimum effective price for the 2nd transaction
        t2Profit = max(t2Profit, price -

 t2Cost);       // Max profit for the 2nd transaction
    }
    
    return t2Profit;
}
```

#### Explanation:
- This greedy approach tracks effective costs and profits for at most two transactions.
- `t1Cost` and `t2Cost` track the lowest costs for the first and second transactions.
- `t1Profit` and `t2Profit` keep track of maximum profits for both transactions.

#### Time Complexity:
- **O(n)** for a single traversal of `prices`.

#### Space Complexity:
- **O(1)** for constant space usage.

#### Dry Run:
For `prices = [3,3,5,0,0,3,1,4]`, `solveMera` updates `t1Cost`, `t1Profit`, `t2Cost`, and `t2Profit` at each step, finding the best two transactions.

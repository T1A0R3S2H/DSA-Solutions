
### 1. `solveRec`: Pure Recursion

```cpp
int solveRec(int index, int buy, vector<int>& prices, int k) {
    if (index == prices.size() || k == 0) return 0;

    if (buy) {
        int buyProfit = -prices[index] + solveRec(index + 1, 0, prices, k);
        int skipProfit = 0 + solveRec(index + 1, 1, prices, k);
        return max(buyProfit, skipProfit);
    } else {
        int sellProfit = prices[index] + solveRec(index + 1, 1, prices, k - 1);
        int skipProfit = 0 + solveRec(index + 1, 0, prices, k);
        return max(sellProfit, skipProfit);
    }
}
```

- **Explanation**: 
  - The function tries every possibility to maximize profit: either buying or selling a stock at each day, or skipping.
  - If `buy` is 1, we can either buy a stock or skip.
  - If `buy` is 0, we can sell a stock and reduce `k` (transaction count) by 1, or skip.
  - Base case: if `index` reaches the end of `prices` or `k` transactions are done, return 0 (no more profit possible).

- **Time Complexity**: \(O(2^{n \cdot k})\)  
  - Recursive tree is very large due to repeated calculations for each `index` and `k`.

- **Space Complexity**: \(O(n \cdot k)\)  
  - Stack depth in the worst case with `n` days and `k` transactions.

- **Dry Run**: 
  - For `prices = [2, 4, 1]`, `k = 2`:
    - Start at `index = 0`, `buy = 1` (we can buy):
      - Buy at `index = 0`, go to `index = 1` with `buy = 0`.
      - At `index = 1`, sell for profit 2, reaching max profit of 2.

---

### 2. `solveMem`: Recursion with Memoization

```cpp
int solveMem(int index, int buy, vector<int>& prices, int k, vector<vector<vector<int>>>& dp) {
    if (index == prices.size() || k == 0) return 0;
    if (dp[index][buy][k] != -1) return dp[index][buy][k];

    if (buy) {
        int buyProfit = -prices[index] + solveMem(index + 1, 0, prices, k, dp);
        int skipProfit = 0 + solveMem(index + 1, 1, prices, k, dp);
        return dp[index][buy][k] = max(buyProfit, skipProfit);
    } else {
        int sellProfit = prices[index] + solveMem(index + 1, 1, prices, k - 1, dp);
        int skipProfit = 0 + solveMem(index + 1, 0, prices, k, dp);
        return dp[index][buy][k] = max(sellProfit, skipProfit);
    }
}
```

- **Explanation**:
  - Similar to `solveRec`, but stores computed results in `dp` to avoid redundant calculations.
  - `dp[index][buy][k]` holds the result for a given day, buy/sell choice, and transaction count.

- **Time Complexity**: \(O(n \cdot 2 \cdot k) = O(n \cdot k)\)
  - Each state `index, buy, k` is calculated once.

- **Space Complexity**: \(O(n \cdot 2 \cdot k) = O(n \cdot k)\)
  - Extra space for the memoization table `dp`.

- **Dry Run**:
  - With `prices = [2, 4, 1]` and `k = 2`, values are stored in `dp`, significantly reducing recursion tree size.

---

### 3. `solveTab`: Tabulation

```cpp
int solveTab(vector<int>& prices, int k) {
    int n = prices.size();
    vector<vector<vector<int>>> dp(n + 1, vector<vector<int>>(2, vector<int>(k + 1, 0)));

    for (int index = n - 1; index >= 0; --index) {
        for (int buy = 0; buy <= 1; ++buy) {
            for (int limit = 1; limit <= k; ++limit) {
                if (buy) {
                    dp[index][buy][limit] = max(-prices[index] + dp[index + 1][0][limit], dp[index + 1][1][limit]);
                } else {
                    dp[index][buy][limit] = max(prices[index] + dp[index + 1][1][limit - 1], dp[index + 1][0][limit]);
                }
            }
        }
    }
    return dp[0][1][k];
}
```

- **Explanation**:
  - A bottom-up approach where we iteratively fill `dp` table from the last day back to the first.
  - `dp[index][buy][limit]` represents the maximum profit from day `index` with a given `buy` status and `limit` remaining transactions.

- **Time Complexity**: \(O(n \cdot 2 \cdot k) = O(n \cdot k)\)

- **Space Complexity**: \(O(n \cdot 2 \cdot k) = O(n \cdot k)\)

- **Dry Run**:
  - With `prices = [3, 2, 6, 5, 0, 3]` and `k = 2`, `dp` is filled iteratively to find the max profit 7.

---

### 4. `solveSO`: Space-Optimized Tabulation

```cpp
int solveSO(vector<int>& prices, int k) {
    int n = prices.size();
    vector<vector<int>> next(2, vector<int>(k + 1, 0)), curr(2, vector<int>(k + 1, 0));

    for (int index = n - 1; index >= 0; --index) {
        for (int buy = 0; buy <= 1; ++buy) {
            for (int limit = 1; limit <= k; ++limit) {
                if (buy) {
                    curr[buy][limit] = max(-prices[index] + next[0][limit], next[1][limit]);
                } else {
                    curr[buy][limit] = max(prices[index] + next[1][limit - 1], next[0][limit]);
                }
            }
        }
        next = curr;
    }
    return next[1][k];
}
```

- **Explanation**:
  - Uses two arrays `curr` and `next` to hold values for current and next day calculations.
  - Reduces space complexity by only keeping track of `curr` and `next`.

- **Time Complexity**: \(O(n \cdot 2 \cdot k) = O(n \cdot k)\)

- **Space Complexity**: \(O(2 \cdot k) = O(k)\)

- **Dry Run**:
  - With `prices = [3, 2, 6, 5, 0, 3]` and `k = 2`, iterating backward with two arrays yields max profit of 7 with optimized space.

---

### Main Function: `maxProfit`

```cpp
int maxProfit(int k, vector<int>& prices) {
    int n = prices.size();
    vector<vector<vector<int>>> dp(n, vector<vector<int>>(2, vector<int>(k + 1, -1)));

    // Use solveMem, solveTab, or solveSO based on preference
    return solveTab(prices, k);
}
```

- **Explanation**:
  - The main function initializes `dp` for `solveMem`.
  - It can call any preferred approach (`solveRec`, `solveMem`, `solveTab`, or `solveSO`).

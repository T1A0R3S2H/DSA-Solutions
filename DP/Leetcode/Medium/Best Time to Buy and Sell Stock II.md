
### 1. `solveMera` - Greedy Approach

#### Code
```cpp
int solveMera(vector<int>&prices){
    int profit=0;
    for (int i=1; i<prices.size(); i++) {
        if (prices[i]>prices[i-1]) {
            profit+=(prices[i]-prices[i-1]);
        }
    }
    return profit;
}
```

#### Explanation
This approach iterates through the price array and adds to `profit` every time there is a positive difference between consecutive days. This simulates buying on one day and selling on the next whenever there’s a profit to be made.

#### Time Complexity
- \(O(n)\), where \(n\) is the length of `prices`.

#### Space Complexity
- \(O(1)\), as it uses only a single variable `profit`.

#### Dry Run
For `prices = [7,1,5,3,6,4]`:
- Day 2: 1 < 7 → profit += 0 (no change)
- Day 3: 5 > 1 → profit += 4 (profit = 4)
- Day 4: 3 < 5 → profit += 0 (no change)
- Day 5: 6 > 3 → profit += 3 (profit = 7)

---

### 2. `solveRec` - Recursive Approach

#### Code
```cpp
int solveRec(int index, int buy, vector<int>& prices) {
    if (index == prices.size()) return 0;
    
    if (buy) {
        int buyProfit = -prices[index] + solveRec(index + 1, 0, prices);
        int skipProfit = solveRec(index + 1, 1, prices);
        return max(buyProfit, skipProfit);
    } else {
        int sellProfit = prices[index] + solveRec(index + 1, 1, prices);
        int skipProfit = solveRec(index + 1, 0, prices);
        return max(sellProfit, skipProfit);
    }
}
```

#### Explanation
This approach recursively explores every possible transaction on each day, maximizing profit by either buying or skipping if `buy == 1`, or selling or skipping if `buy == 0`.

#### Time Complexity
- \(O(2^n)\), due to exploring all possible states.

#### Space Complexity
- \(O(n)\) for the recursive stack.

#### Dry Run
For `prices = [7,1,5]`:
- Day 1: Buy at 7, skip or sell later.
- Day 2: Skip or buy at 1.
- Day 3: Sell at 5 if previously bought.

---

### 3. `solveMem` - Memoization Approach

#### Code
```cpp
int solveMem(int index, int buy, vector<int>& prices, vector<vector<int>>& dp) {
    if (index == prices.size()) return 0;
    
    if (dp[index][buy] != -1) return dp[index][buy];
    
    if (buy) {
        int buyProfit = -prices[index] + solveMem(index + 1, 0, prices, dp);
        int skipProfit = solveMem(index + 1, 1, prices, dp);
        dp[index][buy] = max(buyProfit, skipProfit);
    } else {
        int sellProfit = prices[index] + solveMem(index + 1, 1, prices, dp);
        int skipProfit = solveMem(index + 1, 0, prices, dp);
        dp[index][buy] = max(sellProfit, skipProfit);
    }
    return dp[index][buy];
}
```

#### Explanation
This uses a DP table `dp[index][buy]` to store results of subproblems, avoiding redundant calculations by storing previously computed profits.

#### Time Complexity
- \(O(n \times 2)\), due to `n` states with `2` choices (buy or sell).

#### Space Complexity
- \(O(n \times 2)\) for the DP table.

#### Dry Run
With `prices = [1,2]`:
- Day 1: `buyProfit = -1 + solveMem(1, 0)`, `skipProfit = solveMem(1, 1)`.
- Day 2: `sellProfit = 2 + solveMem(2, 1)`, `skipProfit = solveMem(2, 0)`.

---

### 4. `solveTab` - Tabulation Approach

#### Code
```cpp
int solveTab(vector<int>& prices) {
    int n = prices.size();
    vector<vector<int>> dp(n + 1, vector<int>(2, 0));
    
    for (int index = n - 1; index >= 0; index--) {
        for (int buy = 0; buy <= 1; buy++) {
            if (buy) {
                int buyProfit = -prices[index] + dp[index + 1][0];
                int skipProfit = dp[index + 1][1];
                dp[index][buy] = max(buyProfit, skipProfit);
            } else {
                int sellProfit = prices[index] + dp[index + 1][1];
                int skipProfit = dp[index + 1][0];
                dp[index][buy] = max(sellProfit, skipProfit);
            }
        }
    }
    return dp[0][1];
}
```

#### Explanation
A bottom-up approach that fills out the DP table iteratively, calculating the maximum profit at each state from the last day to the first day.

#### Time Complexity
- \(O(n \times 2)\), for iterating through all days and both buy/sell states.

#### Space Complexity
- \(O(n \times 2)\), for the DP table.

#### Dry Run
For `prices = [1,3]`:
- Day 2: Populate `dp[1][0]` and `dp[1][1]`.
- Day 1: Populate `dp[0][0]` and `dp[0][1]`.

---

### 5. `solveSO` - Space Optimized Approach

#### Code
```cpp
int solveSO(vector<int>& prices) {
    int n = prices.size();
    vector<int> curr(2, 0), next(2, 0);
    
    for (int index = n - 1; index >= 0; index--) {
        for (int buy = 0; buy <= 1; buy++) {
            if (buy) {
                int buyProfit = -prices[index] + next[0];
                int skipProfit = next[1];
                curr[buy] = max(buyProfit, skipProfit);
            } else {
                int sellProfit = prices[index] + next[1];
                int skipProfit = next[0];
                curr[buy] = max(sellProfit, skipProfit);
            }
        }
        next = curr;
    }
    return curr[1];
}
```

#### Explanation
This further optimizes space by only storing results of the current and next day in two 1D arrays, `curr` and `next`.

#### Time Complexity
- \(O(n \times 2)\), same as tabulation.

#### Space Complexity
- \(O(1)\), using only two 1D arrays of size 2.

#### Dry Run
With `prices = [3,2,6]`:
- Calculate profit for day 3, update `curr` and `next` for day 2.

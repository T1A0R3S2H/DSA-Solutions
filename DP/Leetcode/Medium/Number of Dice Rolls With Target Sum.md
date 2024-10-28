
### 1. `solveRec` (Recursive Approach)

```cpp
long long solveRec(int dice, int faces, int target) {
    // base cases
    if (target < 0) return 0;
    if (target != 0 && dice == 0) return 0;
    if (target == 0 && dice != 0) return 0;
    if (target == 0 && dice == 0) return 1;

    long long ans = 0;
    for (int i = 1; i <= faces; i++) {
        if (target - i >= 0) {
            ans = (ans + solveRec(dice - 1, faces, target - i)) % MOD;
        }
    }
    return ans;
}
```

#### Explanation
The `solveRec` function uses recursion to count the ways to achieve a target sum with a given number of dice:
- If the `target` is less than 0, return 0 (no way to achieve a negative target).
- If there are no dice left and the target is not zero, return 0.
- If the target is 0 and there are no dice left, return 1 (one way to achieve 0).
- Iterate through each face of the dice (1 to `faces`), recursively calling `solveRec` for the next die and the remaining target.

#### Time Complexity
- \(O(k^n)\), where \(k\) is the number of faces and \(n\) is the number of dice.

#### Space Complexity
- \(O(n)\) due to the recursion stack.

#### Dry Run
**Input**: `solveRec(2, 6, 7)`

- Call `solveRec(2, 6, 7)`.
- For each face (1 to 6), call recursively until the base cases are reached.
- Each recursive call reduces the number of dice and the target, accumulating the number of valid ways to reach the target.

---

### 2. `solveMem` (Memoization Approach)

```cpp
long long solveMem(int dice, int faces, int target, vector<vector<int>>& dp) {
    // Base cases
    if (target < 0 || dice < 0) return 0;
    if (dice == 0) return target == 0;

    if (dp[dice][target] != -1) return dp[dice][target];

    long long ans = 0;
    for (int i = 1; i <= faces; i++) {
        if (target - i >= 0) {
            ans = (ans + solveMem(dice - 1, faces, target - i, dp)) % MOD;
        }
    }
    dp[dice][target] = ans;
    return ans;
}
```

#### Explanation
The `solveMem` function enhances the recursive approach by storing results of computed states in a DP table to avoid redundant calculations:
- Check for base cases similar to `solveRec`.
- If a result for the current state is already computed, return it.
- Otherwise, compute the result using recursion and store it in the `dp` table.

#### Time Complexity
- \(O(n \times t)\), where \(n\) is the number of dice and \(t\) is the target value.

#### Space Complexity
- \(O(n \times t)\) for the DP table.

#### Dry Run
**Input**: `solveMem(2, 6, 7)` with `dp` initialized to -1.

- Call `solveMem(2, 6, 7)`.
- Check `dp[2][7]` (initially -1), compute using faces 1 to 6.
- Store results in `dp` to avoid recalculating the same states.

---

### 3. `solveTab` (Tabulation Approach)

```cpp
long long solveTab(int dice, int faces, int target) {
    vector<vector<long long>> dp(dice + 1, vector<long long>(target + 1, 0));
    dp[0][0] = 1; // One way to achieve target 0 with 0 dice

    for (int d = 1; d <= dice; d++) {
        for (int t = 0; t <= target; t++) {
            for (int f = 1; f <= faces; f++) {
                if (t - f >= 0) {
                    dp[d][t] = (dp[d][t] + dp[d - 1][t - f]) % MOD;
                }
            }
        }
    }
    return dp[dice][target];
}
```

#### Explanation
The `solveTab` function employs a bottom-up approach to fill a DP table iteratively:
- Initialize the DP table, setting `dp[0][0] = 1`.
- For each die and each possible target, compute the number of ways to reach that target by summing results from the previous dice.

#### Time Complexity
- \(O(n \times t \times k)\), where \(n\) is the number of dice, \(t\) is the target, and \(k\) is the number of faces.

#### Space Complexity
- \(O(n \times t)\) for the DP table.

#### Dry Run
**Input**: `solveTab(2, 6, 7)`

- Initialize a DP table.
- Iterate through each die and target, updating the table based on previous results.
- Final result is found in `dp[2][7]`.

---

### 4. `solveSO` (Space Optimization Approach)

```cpp
long long solveSO(int dice, int faces, int target) {
    vector<long long> prev(target + 1, 0);
    vector<long long> curr(target + 1, 0);
    prev[0] = 1; // One way to achieve target 0 with 0 dice

    for (int d = 1; d <= dice; d++) {
        for (int t = 0; t <= target; t++) {
            curr[t] = 0; // Reset current row
            for (int f = 1; f <= faces; f++) {
                if (t - f >= 0) {
                    curr[t] = (curr[t] + prev[t - f]) % MOD;
                }
            }
        }
        swap(prev, curr); // Move to the next dice
    }
    return prev[target]; // Return the result for the last dice
}
```

#### Explanation
The `solveSO` function optimizes space by using only two arrays (previous and current) instead of a full DP table:
- Initialize the previous array to represent base cases.
- Iterate through the number of dice, updating the current array based on the previous results.
- After processing each die, swap the arrays to continue to the next.

#### Time Complexity
- \(O(n \times t \times k)\), where \(n\) is the number of dice, \(t\) is the target, and \(k\) is the number of faces.

#### Space Complexity
- \(O(t)\) because it only uses two arrays of size equal to the target.

#### Dry Run
**Input**: `solveSO(2, 6, 7)`

- Initialize `prev` and `curr` arrays.
- Set `prev[0] = 1`.
- For each die, reset `curr` and update it based on `prev`.
- The final value in `prev[target]` gives the result.

---

This detailed overview should provide you with a clear understanding of each function, including their logic, complexity, and execution flow.

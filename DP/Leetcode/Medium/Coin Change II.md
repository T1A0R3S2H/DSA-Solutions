### TL;DR:
Use the **take-and-notTake** approach to recursively explore all combinations of coins that sum up to the target amount. Optimize it using memoization to store intermediate results. Base cases handle valid combinations (`amount == 0`) and invalid states (`amount < 0` or out of bounds).

---

### CETSD:

#### Code:
```cpp
class Solution {
public:
    int solveMem(int index, int amount, vector<int>& coins, vector<vector<int>>& dp) {
        // Base Cases
        if (amount==0) return 1;
        if (index==coins.size() || amount<0) return 0;
        if (dp[index][amount]!=-1) return dp[index][amount];

        int take=solveMem(index, amount-coins[index], coins, dp);
        int notTake=solveMem(index+1, amount, coins, dp);
        return dp[index][amount]=take+notTake;
    }

    int change(int amount, vector<int>& coins) {
        vector<vector<int>> dp(coins.size(), vector<int>(amount+1, -1));
        return solveMem(0, amount, coins, dp);
    }
};
```

---

#### Explanation:
1. **Recursive Logic**:
   - `take`: Include the current coin in the combination by reducing the target `amount` and staying on the same index.
   - `notTake`: Skip the current coin and move to the next index without changing the `amount`.

2. **Base Cases**:
   - **Valid Combination**: If `amount == 0`, return `1` because one valid way is found.
   - **Invalid State**: If `amount < 0` or `index == coins.size()` (out of bounds), return `0`.

3. **Memoization**:
   - Use a `dp` table to store results for each state defined by `(index, amount)`.
   - Prevent redundant calculations by directly returning results from `dp` when available.

---

#### Time Complexity:
- **O(n * amount)**: Each state (`index`, `amount`) is computed at most once due to memoization.

#### Space Complexity:
- **O(n * amount)**: For the `dp` array storing intermediate results.
- **O(n)**: Maximum depth of the recursion stack (equal to the number of coins).

---

### Dry Run:
---

### Input:
- `coins = [1, 2, 5]`
- `amount = 5`

---

### Execution Steps:

1. **Initial Call**:
   - `solveMem(0, 5)`
   - Two choices:
     - `take`: Use coin `1` → `solveMem(0, 4)`
     - `notTake`: Skip coin `1` → `solveMem(1, 5)`

---

#### **Branch 1: `take` → `solveMem(0, 4)`**
   - Two choices:
     - `take`: Use coin `1` again → `solveMem(0, 3)`
     - `notTake`: Skip coin `1` → `solveMem(1, 4)`

---

##### **Branch 1.1: `take` → `solveMem(0, 3)`**
   - Two choices:
     - `take`: Use coin `1` again → `solveMem(0, 2)`
     - `notTake`: Skip coin `1` → `solveMem(1, 3)`

---

###### **Branch 1.1.1: `take` → `solveMem(0, 2)`**
   - Two choices:
     - `take`: Use coin `1` again → `solveMem(0, 1)`
     - `notTake`: Skip coin `1` → `solveMem(1, 2)`

---

####### **Branch 1.1.1.1: `take` → `solveMem(0, 1)`**
   - Two choices:
     - `take`: Use coin `1` again → `solveMem(0, 0)` (**Valid Combination!** Return `1`).
     - `notTake`: Skip coin `1` → `solveMem(1, 1)`

---

**Branch 1.1.1.1.1**: `solveMem(0, 0)` → Return `1` (Base Case: Valid combination).

**Branch 1.1.1.1.2**: `solveMem(1, 1)` → No valid combinations with `coins = [2, 5]`. Return `0`.

**Result of `solveMem(0, 1)`** = `1 + 0 = 1`.

---

####### **Branch 1.1.1.2: `notTake` → `solveMem(1, 2)`**
   - Two choices:
     - `take`: Use coin `2` → `solveMem(1, 0)` (**Valid Combination!** Return `1`).
     - `notTake`: Skip coin `2` → `solveMem(2, 2)`.

---

**Branch 1.1.1.2.1**: `solveMem(1, 0)` → Return `1` (Base Case: Valid combination).

**Branch 1.1.1.2.2**: `solveMem(2, 2)` → No valid combinations with `coins = [5]`. Return `0`.

**Result of `solveMem(1, 2)`** = `1 + 0 = 1`.

**Result of `solveMem(0, 2)`** = `1 + 1 = 2`.

---

###### **Branch 1.1.2: `notTake` → `solveMem(1, 3)`**
   - Two choices:
     - `take`: Use coin `2` → `solveMem(1, 1)` (**Already Calculated**: Return `0`).
     - `notTake`: Skip coin `2` → `solveMem(2, 3)`.

---

**Branch 1.1.2.1**: `solveMem(1, 1)` → Return `0`.

**Branch 1.1.2.2**: `solveMem(2, 3)` → No valid combinations with `coins = [5]`. Return `0`.

**Result of `solveMem(1, 3)`** = `0 + 0 = 0`.

**Result of `solveMem(0, 3)`** = `2 + 0 = 2`.

---

##### **Branch 1.2: `notTake` → `solveMem(1, 4)`**
   - Two choices:
     - `take`: Use coin `2` → `solveMem(1, 2)` (**Already Calculated**: Return `1`).
     - `notTake`: Skip coin `2` → `solveMem(2, 4)`.

---

**Branch 1.2.1**: `solveMem(1, 2)` → Return `1`.

**Branch 1.2.2**: `solveMem(2, 4)` → No valid combinations with `coins = [5]`. Return `0`.

**Result of `solveMem(1, 4)`** = `1 + 0 = 1`.

**Result of `solveMem(0, 4)`** = `2 + 1 = 3`.

---

#### **Branch 2: `notTake` → `solveMem(1, 5)`**
   - Two choices:
     - `take`: Use coin `2` → `solveMem(1, 3)` (**Already Calculated**: Return `0`).
     - `notTake`: Skip coin `2` → `solveMem(2, 5)`.

---

**Branch 2.1**: `solveMem(1, 3)` → Return `0`.

**Branch 2.2**: `solveMem(2, 5)` → Use coin `5` → `solveMem(2, 0)` (**Valid Combination! Return `1`).

**Result of `solveMem(1, 5)`** = `0 + 1 = 1`.

---

**Final Result of `solveMem(0, 5)`** = `3 + 1 = 4`.

---

### Memoization Table (`dp`):

| Index\Amount | 0  | 1  | 2  | 3  | 4  | 5  |
|--------------|-----|-----|-----|-----|-----|-----|
| 0            | 1   | 1   | 2   | 2   | 3   | 4   |
| 1            | 1   | 0   | 1   | 0   | 1   | 1   |
| 2            | 1   | 0   | 0   | 0   | 0   | 1   |

---


The **output** is returned from `dp[0][5]`, the top-leftmost recursive call for the complete problem. Here's how the value is derived and represents the final answer:

---

#### Explanation of the DP Table:
| Index\Amount | 0  | 1  | 2  | 3  | 4  | 5  |
|--------------|-----|-----|-----|-----|-----|-----|
| 0            | 1   | 1   | 2   | 2   | 3   | **4** |
| 1            | 1   | 0   | 1   | 0   | 1   | 1     |
| 2            | 1   | 0   | 0   | 0   | 0   | 1     |

1. The DP table shows the number of combinations for each `(index, amount)` pair.
2. `dp[index][amount]` gives the total combinations possible to form `amount` using coins starting from `index`.
3. **Final Output**:
   - `dp[0][5]` = **4**.
   - This is computed as the sum of:
     - All combinations when **taking** coins starting at `index 0` (coin `1`).
     - All combinations when **skipping** coins starting at `index 0` (moving to the next coins).

---

### Returned Output:
The algorithm returns `dp[0][5] = 4`. This represents the total number of unique combinations to make the amount `5` using the coins `[1, 2, 5]`.
### Final Answer:
The total number of combinations is **4**:
1. `5`
2. `2 + 2 + 1`
3. `2 + 1 + 1 + 1`
4. `1 + 1 + 1 + 1 + 1`.

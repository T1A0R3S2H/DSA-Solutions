### 1. Recursive Approach: `solveRec`

#### Code
```cpp
bool solveRec(int index, vector<int>& nums, int target) {
    int n = nums.size();
    if (index >= n) return false;
    if (target == 0) return true;
    if (target < 0) return false;

    bool inc = solveRec(index + 1, nums, target - nums[index]);
    bool exc = solveRec(index + 1, nums, target);

    return inc || exc;
}
```

#### Explanation
The recursive function `solveRec` checks if we can achieve the target sum by including or excluding elements starting from `index`. 
1. **Base cases**:
   - If `index` goes out of bounds (`index >= n`), return `false`.
   - If `target` reaches zero, return `true`.
   - If `target` is negative, return `false`.
2. For each element at `index`, it explores two paths:
   - **Include**: Recurse with `target - nums[index]`.
   - **Exclude**: Recurse without changing `target`.
3. If either path returns `true`, it indicates a valid subset sum exists.

#### Time Complexity
- **O(2^n)**: Each element can either be included or excluded, leading to `2^n` possible combinations.

#### Space Complexity
- **O(n)**: Stack space for recursion up to `n` levels deep.

#### Dry Run
Input: `nums = [1, 5, 11, 5]`, `target = 11`
1. Start with `index = 0`, `target = 11`.
2. Recursive calls branch into inclusion and exclusion paths:
   - **Include `nums[0] = 1`**:
     - New target is `10`.
     - Continue down this path.
   - **Exclude `nums[0] = 1`**:
     - Target remains `11`.
3. This branching continues recursively, checking each path until `target = 0` is achieved by selecting `[5, 5, 1]` subset.

---

### 2. Memoized Approach: `solveMem`

#### Code
```cpp
bool solveMem(int index, vector<int>& nums, int target, vector<vector<int>>& dp) {
    if (target == 0) return true;
    if (index >= nums.size() || target < 0) return false;

    if (dp[index][target] != -1) return dp[index][target];

    bool inc = solveMem(index + 1, nums, target - nums[index], dp);
    bool exc = solveMem(index + 1, nums, target, dp);

    return dp[index][target] = inc || exc;
}
```

#### Explanation
This memoized version optimizes the recursion by storing results in `dp`.
1. **Base Cases**:
   - Same as the recursive solution.
2. **Memoization**:
   - If `dp[index][target]` has a stored result, return it to avoid redundant calculations.
   - Otherwise, calculate inclusion and exclusion as before, and store the result in `dp[index][target]`.

#### Time Complexity
- **O(n * target)**: Each state `(index, target)` is computed only once due to memoization.

#### Space Complexity
- **O(n * target)**: Space required for the `dp` table.
- **O(n)**: Stack space for recursion.

#### Dry Run
Input: `nums = [1, 5, 11, 5]`, `target = 11`
1. Initialize `dp` matrix with `-1`.
2. Proceed with recursive calls as in the previous approach.
3. If the same state `(index, target)` recurs, return the stored `dp` result.
4. This speeds up calculations significantly, especially for large inputs.

---

### 3. Tabulation Approach: `solveTab`

#### Code
```cpp
bool solveTab(vector<int>& nums, int target) {
    int n = nums.size();
    vector<vector<bool>> dp(n + 1, vector<bool>(target + 1, false));

    // Base case: target == 0 is achievable by choosing no elements
    for (int i = 0; i <= n; i++) {
        dp[i][0] = true;
    }

    for (int i = n - 1; i >= 0; i--) {
        for (int t = 1; t <= target; t++) {
            bool inc = false;
            if (t - nums[i]>=0) {
                inc = dp[i + 1][t - nums[i]];
            }
            bool exc = dp[i + 1][t];
            dp[i][t] = inc || exc;
        }
    }

    return dp[0][target];
}

```

#### Explanation
The tabulation approach builds a `dp` table bottom-up to avoid recursion.
1. **Base Case Initialization**:
   - `dp[i][0]` is `true` for all `i`, as a zero target can be achieved by picking no elements.
2. **Filling the Table**:
   - For each index `i` (in reverse), and each target `t`, check:
     - If including `nums[i]` can achieve `t` (`inc`).
     - If excluding `nums[i]` can achieve `t` (`exc`).
   - Store the result of `inc || exc` in `dp[i][t]`.

#### Time Complexity
- **O(n * target)**: Iterates over `n` items and `target` sum.

#### Space Complexity
- **O(n * target)**: Space required for `dp` table.

#### Dry Run
Input: `nums = [1, 5, 11, 5]`, `target = 11`
1. Initialize `dp` table with `n+1` rows and `target+1` columns.
2. For each target `t` from `1` to `11`, fill values bottom-up based on `inc` and `exc` logic.
3. After populating, `dp[0][11]` gives `true`, indicating a subset with sum `11` exists.

---

### 4. Space Optimized Approach: `solveSO`

#### Code
```cpp
bool solveSO(vector<int>& nums) {
    int n=nums.size();
        int total=0;
        for(int i=0; i<n; i++){
            total+=nums[i];
        }
        if(total&1) return false;
        int target=total/2;
    vector<bool> prev(target + 1, false), curr(target + 1, false);

    // Base case: target == 0 is achievable by choosing no elements
    prev[0] = true;

    for (int i = 0; i < n; i++) {
        curr[0] = true;  // Target 0 is always achievable

        for (int t = 1; t <= target; t++) {
            bool inc = false;
            if (t - nums[i]>=0) {
                inc = prev[t - nums[i]];
            }
            bool exc = prev[t];
            curr[t] = inc || exc;
        }

        // Move current row to previous for the next iteration
        prev = curr;
    }

    return prev[target];
}

```

#### Explanation
The space-optimized approach reduces the 2D `dp` table to two 1D arrays.
1. **Initialization**:
   - `prev[0]` is `true` for achieving a zero target.
2. **Iterative DP Update**:
   - For each element in `nums`, update `curr` using `prev`, which stores the results of the previous iteration.
   - After each row, `prev` is updated to `curr`.

#### Time Complexity
- **O(n * target)**: Similar to the tabulation approach.

#### Space Complexity
- **O(target)**: Only two 1D arrays of length `target + 1` are used.

#### Dry Run
Input: `nums = [1, 5, 11, 5]`, `target = 11`
1. Initialize `prev` with `prev[0] = true`.
2. Update `curr` using values from `prev` for each element in `nums`.
3. The value `prev[target]` indicates if the subset sum `11` is possible.

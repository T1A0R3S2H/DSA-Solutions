

### 1. `solveRec`

```cpp
int solveRec(int index, int diff, vector<int>& nums) {
    if (index < 0) return 0;
    int ans = 0;
    for (int j = index - 1; j >= 0; j--) {
        if (nums[index] - nums[j] == diff) {
            ans = max(ans, 1 + solveRec(j, diff, nums));
        }
    }
    return ans;
}
```

- **Explanation**:  
  `solveRec` is a recursive function that finds the length of the longest arithmetic subsequence ending at `index` with a common difference `diff`. It checks each preceding element `j` and recursively computes the longest subsequence ending at `j` with the same difference. If `nums[index] - nums[j] == diff`, it extends the sequence length by `1` and calls itself on `j`.

- **Time Complexity**: O(2^n), as the function makes recursive calls for each index, leading to exponential time complexity.

- **Space Complexity**: O(n), for the recursive call stack depth in the worst case.

- **Dry Run**:
  ```cpp
  nums = [3, 6, 9, 12], diff = 3
  Call: solveRec(3, 3, nums)
      -> Call: solveRec(2, 3, nums)
          -> Call: solveRec(1, 3, nums)
              -> Call: solveRec(0, 3, nums) // returns 0 (base case)
              -> ans = max(0, 1 + 0) = 1
          -> ans = max(0, 1 + 1) = 2
      -> ans = max(0, 1 + 2) = 3
  Result: 3
  ```

---

### 2. `solveMem`

```cpp
int solveMem(int index, int diff, vector<int>& nums, vector<vector<int>>& dp) {
    if (index < 0) return 0;
    if (dp[index][diff + 500] != -1) return dp[index][diff + 500];
    int ans = 0;
    for (int j = index - 1; j >= 0; j--) {
        if (nums[index] - nums[j] == diff) {
            ans = max(ans, 1 + solveMem(j, diff, nums, dp));
        }
    }
    return dp[index][diff + 500] = ans;
}
```

- **Explanation**:  
  `solveMem` is a memoized version of `solveRec` that uses a `dp` table to store previously computed values. The `dp` array is indexed with `diff + 500` to map negative differences into a valid range. It checks if `dp[index][diff + 500]` has already been computed to avoid redundant calculations, thus improving efficiency.

- **Time Complexity**: O(n^2), since each recursive call now only happens once per unique `(index, diff)` pair.

- **Space Complexity**: O(n * d), where `d` is the range of differences (from `-500` to `500`), for the `dp` array and recursive stack.

- **Dry Run**:
  ```cpp
  nums = [9, 4, 7, 2, 10], diff = 3, dp initialized with -1
  Call: solveMem(4, 3, nums, dp)
      -> solveMem(3, 3, nums, dp) (stores results in dp[3][503])
      -> solveMem(2, 3, nums, dp) (stores results in dp[2][503])
      -> solveMem(1, 3, nums, dp) (stores results in dp[1][503])
      -> solveMem(0, 3, nums, dp) // returns 0
  Updates dp table and returns max length.
  ```

---

### 3. `solveTab`

```cpp
int solveTab(vector<int>& nums) {
    int n = nums.size();
    vector<vector<int>> dp(n, vector<int>(1001, 0));
    int ans = 0;
    for (int index = 0; index < n; index++) {
        for (int j = 0; j < index; j++) {
            int diff = nums[index] - nums[j] + 500;
            dp[index][diff] = max(dp[index][diff], dp[j][diff] + 1);
            ans = max(ans, dp[index][diff]);
        }
    }
    return ans + 1;
}
```

- **Explanation**:  
  `solveTab` is a tabulation-based dynamic programming solution. It uses a 2D `dp` table where `dp[index][diff]` stores the length of the longest arithmetic subsequence ending at `index` with the difference `diff`. For each pair of indices `(j, index)`, it calculates `diff` and updates `dp[index][diff]` based on `dp[j][diff]`. The result `ans + 1` is returned, adding `1` to account for the initial element in each sequence.

- **Time Complexity**: O(n^2), where `n` is the size of `nums`, as we compute each `(index, diff)` pair once.

- **Space Complexity**: O(n * d), where `d` is the range of differences (from `-500` to `500`), for the `dp` table.

- **Dry Run**:
  ```cpp
  nums = [3, 6, 9, 12]
  dp = vector<vector<int>> of size (n x 1001) initialized to 0
  Loop through each pair (j, index):
      For (index = 1, j = 0):
          diff = 6 - 3 + 500 = 503
          dp[1][503] = max(dp[1][503], dp[0][503] + 1) -> dp[1][503] = 1
      For (index = 2, j = 0 and j = 1), calculate diffs and update dp
      Update ans based on dp values.
  Result: 4
  ```

---

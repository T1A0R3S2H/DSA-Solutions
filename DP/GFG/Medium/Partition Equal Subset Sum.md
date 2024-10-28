### Code
```cpp
class Solution {
public:
    // Helper function for the recursive approach
    bool solveRec(int index, int target, int arr[]) {
        if (target == 0) return true; // Found a subset
        if (index == 0) return arr[0] == target; // Only one element to consider

        bool exclude = solveRec(index - 1, target, arr); // Exclude the current element
        bool include = false;
        if (arr[index] <= target) {
            include = solveRec(index - 1, target - arr[index], arr); // Include the current element
        }

        return include || exclude;
    }

    // Helper function for the memoization approach
    bool solveMem(int index, int target, int arr[], vector<vector<int>>& dp) {
        if (target == 0) return true; // Found a subset
        if (index == 0) return arr[0] == target; // Only one element to consider

        if (dp[index][target] != -1) return dp[index][target];

        bool exclude = solveMem(index - 1, target, arr, dp); // Exclude the current element
        bool include = false;
        if (arr[index] <= target) {
            include = solveMem(index - 1, target - arr[index], arr, dp); // Include the current element
        }

        return dp[index][target] = include || exclude;
    }

    // Helper function for the tabulation approach
    bool solveTab(int N, int arr[], int target) {
        vector<vector<bool>> dp(N + 1, vector<bool>(target + 1, false));
        for (int i = 0; i <= N; i++) {
            dp[i][0] = true; // Target 0 is always achievable
        }
        for (int i = 1; i <= N; i++) {
            for (int t = 1; t <= target; t++) {
                bool exclude = dp[i - 1][t]; // Exclude the current element
                bool include = (arr[i - 1] <= t) ? dp[i - 1][t - arr[i - 1]] : false; // Include the current element
                dp[i][t] = include || exclude;
            }
        }
        return dp[N][target];
    }

    // Helper function for the space-optimized approach
    bool solveSO(int N, int arr[], int target) {
        vector<bool> prev(target + 1, false);
        prev[0] = true; // Target 0 is always achievable

        for (int i = 0; i < N; i++) {
            for (int t = target; t >= arr[i]; t--) {
                prev[t] = prev[t] || prev[t - arr[i]];
            }
        }

        return prev[target];
    }

    int equalPartition(int N, int arr[]) {
        int total = 0;
        for (int i = 0; i < N; i++) {
            total += arr[i];
        }

        if (total % 2 != 0) return 0; // If total sum is odd, cannot partition

        int target = total / 2;

        // Uncomment one of the following lines to use the desired approach
        // return solveRec(N - 1, target, arr) ? 1 : 0; // Recursive approach
        // vector<vector<int>> dp(N, vector<int>(target + 1, -1));
        // return solveMem(N - 1, target, arr, dp) ? 1 : 0; // Memoization approach
        // return solveTab(N, arr, target) ? 1 : 0; // Tabulation approach
        return solveSO(N, arr, target) ? 1 : 0; // Space-optimized approach
    }
};
```
### 1. `solveRec`

#### Explanation
- This is a recursive approach to determine if a subset with a given target sum can be formed from the array.
- It checks:
  - If the target sum is 0, which means a valid subset has been found.
  - If the index is 0, it checks if the first element is equal to the target.
- It explores two options for each element: excluding it from the subset or including it if it does not exceed the target.

#### Time Complexity
- **O(2^N)**: This arises because each element can either be included or excluded, leading to exponential combinations.

#### Space Complexity
- **O(N)**: This is the maximum depth of the recursion stack.

#### Dry Run
Let’s consider `arr = [1, 5, 11, 5]` with `target = 11`:
1. The function is initially called as `solveRec(3, 11, arr)`.
2. It checks:
   - Exclude 5: Calls `solveRec(2, 11, arr)`.
   - Include 5 (if valid): Calls `solveRec(2, 6, arr)`.
3. This continues recursively, exploring all subsets until it finds that including `5` twice leads to the target.

---

### 2. `solveMem`

#### Explanation
- This function uses memoization to optimize the recursive approach by storing results of previously computed states in a 2D `dp` array.
- The base cases remain the same as in `solveRec`.
- If a state has been computed, it retrieves the result from `dp` instead of recalculating.

#### Time Complexity
- **O(N * target)**: Each state is computed only once, leading to a polynomial number of combinations.

#### Space Complexity
- **O(N * target)**: The `dp` array stores results for all states.

#### Dry Run
With the same `arr = [1, 5, 11, 5]` and `target = 11`:
1. The function starts by initializing a `dp` table of size `(N+1) x (target+1)`.
2. It follows the same recursive checks but fills in `dp` to avoid recomputation.
3. It eventually finds that the target can be achieved by retrieving the computed value from `dp`.

---

### 3. `solveTab`

#### Explanation
- This function implements the tabulation approach, filling in a DP table iteratively.
- It initializes a 2D array `dp` where `dp[i][t]` indicates whether a subset of the first `i` elements can achieve the sum `t`.
- The first column is initialized to `true` because a sum of `0` is always achievable.

#### Time Complexity
- **O(N * target)**: The function iterates through each element and possible target sum.

#### Space Complexity
- **O(N * target)**: A 2D `dp` table is used to store results.

#### Dry Run
Using `arr = [1, 5, 11, 5]` with `target = 11`:
1. Initialize `dp` as a 2D array with dimensions `(N+1) x (target+1)`.
2. Set `dp[i][0] = true` for all `i`.
3. Iteratively fill the `dp` table:
   - For each element, check if it can be included or excluded to form the target.
4. The final result is found in `dp[N][target]`.

---

### 4. `solveSO`

#### Explanation
- This function uses a space-optimized approach, reducing the 2D DP table to a 1D array, `prev`, which keeps track of achievable sums.
- It starts by initializing `prev[0]` to `true`, then iterates through the elements of the array and updates `prev` in reverse order to avoid overwriting results from the current iteration.

#### Time Complexity
- **O(N * target)**: Similar to the tabulation approach, but optimized for space.

#### Space Complexity
- **O(target)**: Only a 1D array is used.

#### Dry Run
With `arr = [1, 5, 11, 5]` and `target = 11`:
1. Initialize `prev` with size `target + 1` and set `prev[0] = true`.
2. For each element, update `prev` from `target` down to the value of the current element:
   - For `1`: Update `prev[1] = true`.
   - For `5`: Update `prev[5] = true`, `prev[6] = true`, etc.
   - For `11`: Update `prev[11] = true`.
3. The final check `prev[target]` determines if the target can be achieved.

---

### 5. `equalPartition`

#### Explanation
- This is the main function that calculates the total sum of the array.
- It checks if the total sum is odd; if so, it immediately returns 0 since partitioning is impossible.
- If the sum is even, it sets the target as half the total sum and calls one of the solving functions (here, `solveSO`).

#### Time Complexity
- **O(N)** for calculating the total sum of the array, plus the time complexity of the chosen solving method.

#### Space Complexity
- **O(1)** for sum calculation, plus the space complexity of the chosen method.

#### Dry Run
With `arr = [1, 5, 11, 5]`:
1. Calculate `total = 22`.
2. Since `total` is even, set `target = 11`.
3. Call `solveSO(N, arr, target)` to check if the target can be achieved, leading to the final result.

---

### Summary of Complexities

- **Time Complexity**:
  - `solveRec`: O(2^N)
  - `solveMem`: O(N * target)
  - `solveTab`: O(N * target)
  - `solveSO`: O(N * target)
  - `equalPartition`: O(N) + complexity of chosen method

- **Space Complexity**:
  - `solveRec`: O(N)
  - `solveMem`: O(N * target)
  - `solveTab`: O(N * target)
  - `solveSO`: O(target)
  - `equalPartition`: O(1) + complexity of chosen method

This structured breakdown covers each function, its purpose, complexities, and example dry runs for clarity! Let me know if you need any further details.

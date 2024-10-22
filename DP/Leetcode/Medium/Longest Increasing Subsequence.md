### Code

```cpp
class Solution {
public:
    int solveRec(vector<int>& nums, int curr, int prev){
        int n=nums.size();
        if(curr==n) return 0;
        // include
        int take=0;
        if(prev==-1 || nums[curr]>nums[prev]){
            take=1+solveRec(nums, curr+1, curr);
        }
        // exclude
        int notTake=0+solveRec(nums, curr+1, prev);
        return max(take, notTake);
    }

    int solveMem(vector<int>& nums, int curr, int prev, vector<vector<int>>& dp){
        int n=nums.size();
        if(curr==n) return 0;
        if(dp[curr][prev+1]!=-1) return dp[curr][prev+1];
        // include
        int take=0;
        if(prev==-1 || nums[curr]>nums[prev]){
            take=1+solveMem(nums, curr+1, curr, dp);
        }
        // exclude
        int notTake=0+solveMem(nums, curr+1, prev, dp);
        return dp[curr][prev+1]=max(take, notTake);
    }

    int solveTab(vector<int>& nums){
        int n=nums.size();
        vector<vector<int>> dp(n+1, vector<int>(n+1, 0));
        for(int curr=n-1; curr>=0; curr--){
            for(int prev=curr-1; prev>=-1; prev--){
                // include
                int take=0;
                if(prev==-1 || nums[curr]>nums[prev]){
                    take=1+dp[curr+1][curr+1];
                }
                // exclude
                int notTake=0+dp[curr+1][prev+1];
                dp[curr][prev+1]=max(take, notTake);
            }
        }
        return dp[0][0];
    }

    int solveSpaceOptimized(vector<int>& nums) {
        int n = nums.size();
        vector<int> nextRow(n + 1, 0); // Initialize nextRow
        vector<int> currRow(n + 1, 0); // Initialize currRow

        for(int curr = n - 1; curr >= 0; curr--) {
            for(int prev = curr - 1; prev >= -1; prev--) {
                // include
                int take = 0;
                if(prev == -1 || nums[curr] > nums[prev]) {
                    take = 1 + nextRow[curr + 1];
                }
                // exclude
                int notTake = nextRow[prev + 1];
                currRow[prev + 1] = max(take, notTake);
            }
            nextRow = currRow; // Move to the next row
        }
        return nextRow[0]; // Return the result from the first column
    }

    int lengthOfLIS(vector<int>& nums) {
        int n=nums.size();
        // return solveRec(nums, 0, -1);
        // vector<vector<int>> dp(n+1, vector<int>(n+1, -1));
        // return solveMem(nums, 0, -1, dp);
        // return solveTab(nums);
        return solveSpaceOptimized(nums); // Call the space-optimized function
    }
};
```

### Explanation (CETSD)

1. **Code**: The above implementation contains four functions: 
   - `solveRec`: Recursive solution.
   - `solveMem`: Memoized solution.
   - `solveTab`: Tabulation (DP) solution.
   - `solveSpaceOptimized`: Space-optimized solution.

2. **Explanation**:
   - The goal is to find the length of the longest increasing subsequence in the array `nums`.
   - **Recursive Approach**: Recursively checks whether to include the current number in the increasing subsequence or not.
   - **Memoization**: Saves previously computed results in a DP table to avoid recomputation.
   - **Tabulation**: Iteratively fills up a DP table from the bottom up.
   - **Space Optimization**: Uses two arrays to save space, updating results from the previous iteration.

3. **Time Complexity**:
   - **Recursive**: \(O(2^n)\) due to overlapping subproblems.
   - **Memoization**: \(O(n^2)\) because of the two-dimensional DP table.
   - **Tabulation**: \(O(n^2)\) with a full DP table.
   - **Space Optimized**: \(O(n)\) for the two arrays.

4. **Space Complexity**:
   - **Recursive**: \(O(n)\) for recursion stack.
   - **Memoization**: \(O(n^2)\) for the DP table.
   - **Tabulation**: \(O(n^2)\) for the DP table.
   - **Space Optimized**: \(O(n)\) for the two arrays.

### Dry Run for `solveSpaceOptimized`

#### Input
```cpp
nums = [10, 9, 2, 5, 3, 7, 101, 18]
```
#### Execution Steps

1. **Initialization**: 
   - `n = 8`
   - `nextRow = [0, 0, 0, 0, 0, 0, 0, 0, 0]`
   - `currRow = [0, 0, 0, 0, 0, 0, 0, 0, 0]`

2. **Outer Loop** (`curr` from `7` to `0`):
   - For each `curr`, iterate `prev` from `curr-1` down to `-1`.

#### Iteration Breakdown
- **For `curr = 7` (value = 18)**:
    - `prev = 6`: (prev = 5): 
        - `take = 0` (because `prev` is out of bounds)
        - `currRow[6] = max(1, 0) = 1`
    - `prev = 5`: 
        - `take = 0`
        - `currRow[5] = max(1, 0) = 1`
    - `prev = 4`: 
        - `take = 0`
        - `currRow[4] = max(1, 0) = 1`
    - `prev = 3`: 
        - `take = 0`
        - `currRow[3] = max(1, 0) = 1`
    - `prev = 2`: 
        - `take = 0`
        - `currRow[2] = max(1, 0) = 1`
    - `prev = 1`: 
        - `take = 0`
        - `currRow[1] = max(1, 0) = 1`
    - `prev = 0`: 
        - `take = 0`
        - `currRow[0] = max(1, 0) = 1`
    
    After this step: 
    - `nextRow = [1, 1, 1, 1, 1, 1, 1, 1, 0]`

- **For `curr = 6` (value = 101)**:
    - `prev = 5` to `-1`: 
        - `take = 1 + nextRow[7] = 1 + 0 = 1` 
        - `currRow[6] = max(1, 0) = 1`, and similar for others, the `currRow` remains `[1, 1, 1, 1, 1, 1, 1, 1, 0]`.

- **For `curr = 5` (value = 7)**:
    - `prev = 4` to `-1`:
        - Only `currRow[4] = max(2, 1)`, others stay as `1`.

- **For `curr = 4` (value = 3)**:
    - `prev = 3` to `-1`:
        - Here it becomes clearer that a longer subsequence starts to form.

- Continuing this process until `curr = 0` (value = 10), you build up your `currRow`.

Finally, `nextRow[0]` holds the maximum length of the longest increasing subsequence, which would be `4` for the given input.

### Result
The result of `lengthOfLIS(nums)` is **4**, corresponding to the longest increasing subsequence: `[2, 3, 7, 101]`.

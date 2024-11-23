### Code
```cpp
class Solution {
public:
    bool solveMem(vector<int>& nums, vector<int>& dp, int index) {
        // Base cases
        if (index>=nums.size()-1) return true;
        if (nums[index]==0) return false;
        if (dp[index]!=-1) return dp[index];

        int maxJump=nums[index];
        for (int step=1; step<=maxJump; step++) {
            if (solveMem(nums, dp, index+step)) {
                return dp[index]=true;
            }
        }
        return dp[index]=false;
    }
    
    bool canJump(vector<int>& nums) {
        int n=nums.size();
        vector<int> dp(n, -1);
        return solveMem(nums, dp, 0);
    }
};
```

---

#### **Explanation**
1. The `canJump` function initializes the `dp` array and calls `solveMem` starting at index `0`.
2. The `solveMem` function uses memoization:
   - If the current index is at or beyond the last index, return `true`.
   - If the current index has already been computed, return the stored value.
   - Iterate over all possible jumps from the current index (`1` to `nums[index]`) and recursively check if any path reaches the end.
   - Memoize the result for the current index.
3. The process ensures we check all possible paths efficiently while avoiding redundant calculations.

#### **Time Complexity**
- **Recursive function calls**: Each index is visited at most once, and for each index, we consider all possible jumps.  
  - Worst case: \( O(n^2) \) for fully nested loops.
  
- **Optimized approaches (like greedy)** achieve \( O(n) \), but this is a \( O(n^2) \) solution.

#### **Space Complexity**
- **Memoization array**: \( O(n) \)
- **Recursion stack**: \( O(n) \) in the worst case.

Overall space complexity: \( O(n) \).

#### **Dry Run**
Let’s perform a **full dry run** of the code for both example cases to understand the step-by-step execution.


### Example 1:  
**Input**: `nums = [2, 3, 1, 1, 4]`  

#### Initial Setup:
- `n = 5`
- `dp = [-1, -1, -1, -1, -1]` (memoization array)

#### Execution:
1. **Index 0**:
   - `nums[0] = 2`
   - Possible jumps: to indices `1` and `2`.
   - Check index `1` first.
   
2. **Index 1**:
   - `nums[1] = 3`
   - Possible jumps: to indices `2`, `3`, and `4`.
   - Check index `2` first.

3. **Index 2**:
   - `nums[2] = 1`
   - Possible jumps: to index `3`.
   - Check index `3`.

4. **Index 3**:
   - `nums[3] = 1`
   - Possible jumps: to index `4`.
   - Check index `4`.

5. **Index 4**:
   - Reached the last index. Return `true`.
   - Memoize results backward:
     - `dp[3] = true`
     - `dp[2] = true`
     - `dp[1] = true`
     - `dp[0] = true`

**Final Output**: `true`

---

### Example 2:  
**Input**: `nums = [3, 2, 1, 0, 4]`

#### Initial Setup:
- `n = 5`
- `dp = [-1, -1, -1, -1, -1]` (memoization array)

#### Execution:
1. **Index 0**:
   - `nums[0] = 3`
   - Possible jumps: to indices `1`, `2`, and `3`.
   - Check index `1` first.

2. **Index 1**:
   - `nums[1] = 2`
   - Possible jumps: to indices `2` and `3`.
   - Check index `2` first.

3. **Index 2**:
   - `nums[2] = 1`
   - Possible jump: to index `3`.
   - Check index `3`.

4. **Index 3**:
   - `nums[3] = 0`
   - Cannot jump further. Return `false`.

5. Backtrack to index `2`:
   - No other paths available. Memoize `dp[2] = false`.

6. Backtrack to index `1`:
   - Check index `3` next. It’s already computed as `false`. Memoize `dp[1] = false`.

7. Backtrack to index `0`:
   - Check index `2` next. It’s already computed as `false`.
   - Check index `3` next. It’s already computed as `false`.
   - Memoize `dp[0] = false`.

**Final Output**: `false`

---

### Summary of DP Table:
- **Example 1**: `dp = [true, true, true, true, true]`
- **Example 2**: `dp = [false, false, false, false, -1]`

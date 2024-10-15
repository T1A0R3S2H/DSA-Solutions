
```cpp
class Solution {
public:
    // Recursive solution
    int solveRec(vector<int>& nums, int target) {
        if (target == 0) return 1;
        if (target < 0) return 0;
        int count = 0;
        for (int num : nums) {
            count += solveRec(nums, target - num);
        }
        return count;
    }

    // Recursive solution with memoization
    int solveMemo(vector<int>& nums, int target, vector<int>& memo) {
        if (target == 0) return 1;
        if (target < 0) return 0;
        if (memo[target] != -1) return memo[target];
        
        int count = 0;
        for (int num : nums) {
            count += solveMemo(nums, target - num, memo);
        }
        return memo[target] = count;
    }

    // Tabulation solution
    int solveTab(vector<int>& nums, int target) {
        vector<double> dp(target + 1, 0);
        dp[0] = 1;
        for (int i = 1; i <= target; i++) {
            for (int j = 0; j < nums.size(); j++) {
                if (i - nums[j] >= 0) {
                    dp[i] = (dp[i] + dp[i - nums[j]]);
                }
            }
        }
        return (int)dp[target];
    }

    int combinationSum4(vector<int>& nums, int target) {
        // vector<int> memo(target + 1, -1);
        // return solveMemo(nums, target, memo);

        return solveTab(nums, target);
    }
};
```

### Explanation
1. **Recursive Function (`solveRec`)**:
   - This function counts the number of ways to sum up to `target` using the numbers in `nums`. It uses recursion to explore each possibility.

2. **Recursive Function with Memoization (`solveMemo`)**:
   - Similar to the recursive approach, but it stores previously computed results in a memoization array to avoid redundant calculations.

3. **Tabulation Function (`solveTab`)**:
   - This function uses dynamic programming to build up solutions to subproblems iteratively, storing results in a `dp` array.

### Time Complexity
- **Recursive**: O(2^n), where n is the target, as it explores all combinations.
- **Memoization**: O(n * m), where n is the target and m is the size of `nums`.
- **Tabulation**: O(n * m), where n is the target and m is the size of `nums`.

### Space Complexity
- **Recursive**: O(n) for the call stack.
- **Memoization**: O(n) for the memo array.
- **Tabulation**: O(n) for the dp array.

### Dry Run
For example, given `nums = [1, 2, 3]` and `target = 4`:
- **Recursive**: The function will explore all combinations of `1, 2, 3` that sum to `4`, leading to a significant number of function calls.
- **Memoization**: It will cache results for targets already computed, leading to faster execution.
- **Tabulation**: It will fill the `dp` array iteratively, eventually computing the number of ways to achieve the target efficiently. 

Feel free to uncomment the recursive or memoization calls as needed!

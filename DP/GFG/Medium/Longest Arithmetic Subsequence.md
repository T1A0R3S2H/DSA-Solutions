### Code:
```cpp
class Solution {
public:
    // Recursive solution
    int solveRec(int index, int diff, vector<int>& nums) {
        // Base case
        if (index < 0) return 0;
        int ans = 0;
        for (int j = index - 1; j >= 0; j--) {
            if (nums[index] - nums[j] == diff) {
                ans = max(ans, 1 + solveRec(j, diff, nums));
            }
        }
        return ans;
    }

    // Memoized solution
    int solveMem(int index, int diff, vector<int>& nums, vector<vector<int>>& dp) {
        // Base case
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

    // Tabulated solution
    int solveTab(vector<int>& nums) {
        int n = nums.size();
        vector<vector<int>> dp(n, vector<int>(1001, 0));
        int ans = 0;
        
        for (int index = 0; index < n; index++) {
            for (int j = 0; j < index; j++) {
                int diff = nums[index] - nums[j] + 500; // Map diff to non-negative index
                dp[index][diff] = max(dp[index][diff], dp[j][diff] + 1);
                ans = max(ans, dp[index][diff]);
            }
        }
        return ans + 1; // Include the starting element
    }

    // Main function to find the length of the longest arithmetic progression
    int lengthOfLongestAP(vector<int>& arr) {
        int n = arr.size();
        if (n <= 2) return n;

        // Uncomment the desired approach here:
        // 1. Recursive
        /*
        int ans = 0;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                ans = max(ans, 2 + solveRec(i, arr[j] - arr[i], arr));
            }
        }
        return ans;
        */

        // 2. Memoized
        /*
        vector<vector<int>> dp(n, vector<int>(1001, -1));
        int ans = 0;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                ans = max(ans, 2 + solveMem(i, arr[j] - arr[i], arr, dp));
            }
        }
        return ans;
        */

        // 3. Tabulated
        return solveTab(arr);
    }
};
```

### Explanation:
- **Recursive Approach**: This is the naive approach where we recursively find the length of the longest arithmetic progression (AP) by checking all previous indices with the same difference.
- **Memoized Approach**: This improves the recursive solution by storing results in a 2D DP array to avoid redundant calculations.
- **Tabulated Approach**: This builds up solutions iteratively using a 2D DP table, storing counts of subsequences for each possible difference.
- **`lengthOfLongestAP` Function**: This is the main function where you can uncomment the desired approach (recursive, memoized, or tabulated) for testing.

### Testing:
You can test the function with various inputs to verify correctness. If you have any specific test cases or further modifications in mind, feel free to let me know!

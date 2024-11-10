### Code:
```cpp
class Solution {
public:
    int solveRec(string s1, string s2, int i, int j) {
        // base cases
        if (i == s1.length()) return s2.length() - j;
        if (j == s2.length()) return s1.length() - i;

        int ans = 0;
        if (s1[i] == s2[j]) {
            return solveRec(s1, s2, i + 1, j + 1);
        } else {
            int insertAns = 1 + solveRec(s1, s2, i, j + 1); // insert
            int deleteAns = 1 + solveRec(s1, s2, i + 1, j); // delete
            int replaceAns = 1 + solveRec(s1, s2, i + 1, j + 1); // replace

            ans = min(insertAns, min(deleteAns, replaceAns));
        }
        return ans;
    }

    int solveMem(string s1, string s2, int i, int j, vector<vector<int>>& memo) {
        // base cases
        if (i == s1.length()) return s2.length() - j;
        if (j == s2.length()) return s1.length() - i;

        // check memo
        if (memo[i][j] != -1) return memo[i][j];

        int ans = 0;
        if (s1[i] == s2[j]) {
            return memo[i][j] = solveMem(s1, s2, i + 1, j + 1, memo);
        } else {
            int insertAns = 1 + solveMem(s1, s2, i, j + 1, memo); // insert
            int deleteAns = 1 + solveMem(s1, s2, i + 1, j, memo); // delete
            int replaceAns = 1 + solveMem(s1, s2, i + 1, j + 1, memo); // replace

            ans = min(insertAns, min(deleteAns, replaceAns));
        }

        return memo[i][j] = ans;
    }

    int solveTab(string s1, string s2) {
        int m = s1.length();
        int n = s2.length();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1));

        // Base cases
        for (int i = 0; i <= m; i++) dp[i][n] = m - i;
        for (int j = 0; j <= n; j++) dp[m][j] = n - j;

        // Fill the dp table
        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                if (s1[i] == s2[j]) {
                    dp[i][j] = dp[i + 1][j + 1];
                } else {
                    int insertAns = 1 + dp[i][j + 1];
                    int deleteAns = 1 + dp[i + 1][j];
                    int replaceAns = 1 + dp[i + 1][j + 1];

                    dp[i][j] = min(insertAns, min(deleteAns, replaceAns));
                }
            }
        }

        return dp[0][0];
    }

    int solveSO(string s1, string s2) {
        int m = s1.length();
        int n = s2.length();
        vector<int> prev(n + 1), curr(n + 1);

        // Base cases
        for (int j = 0; j <= n; j++) prev[j] = n - j;

        // Fill the dp table row by row
        for (int i = m - 1; i >= 0; i--) {
            curr[n] = m - i;
            for (int j = n - 1; j >= 0; j--) {
                if (s1[i] == s2[j]) {
                    curr[j] = prev[j + 1];
                } else {
                    int insertAns = 1 + curr[j + 1];
                    int deleteAns = 1 + prev[j];
                    int replaceAns = 1 + prev[j + 1];

                    curr[j] = min(insertAns, min(deleteAns, replaceAns));
                }
            }
            prev = curr; // Move to the next row
        }

        return prev[0];
    }

    int editDistance(string s1, string s2) {
        // Call all approaches
        vector<vector<int>> memo(s1.length(), vector<int>(s2.length(), -1));
        // return solveRec(s1, s2, 0, 0);
        // return solveMem(s1, s2, 0, 0, memo);
        return solveTab(s1, s2);
        // return solveSO(s1, s2);
    }
};
```
- **Explanation:**  
  The problem is solved using four approaches:
  1. **Recursive (`solveRec`)**: A naive recursive approach that checks for base cases and recursively computes the minimum number of operations required by considering all three possible operations (insert, delete, replace).
  2. **Memoized (`solveMem`)**: Optimizes the recursive approach by storing intermediate results in a memoization table, reducing redundant calculations.
  3. **Tabulated (`solveTab`)**: Solves the problem iteratively using dynamic programming (bottom-up approach). The table is filled by comparing each character from both strings and calculating the minimum operations.
  4. **Space Optimized (`solveSO`)**: Optimizes space complexity by using only two rows (previous and current) instead of the full DP table.

- **Time Complexity:**
  - **Recursive:** O(3^m) where `m` is the length of `s1` (exponential complexity due to branching at each recursion).
  - **Memoized:** O(m * n) where `m` is the length of `s1` and `n` is the length of `s2` (because each state `(i, j)` is computed only once).
  - **Tabulated:** O(m * n) as the DP table is filled iteratively.
  - **Space Optimized:** O(n) as only two rows are maintained.

- **Space Complexity:**
  - **Recursive:** O(m + n) due to the recursion stack.
  - **Memoized:** O(m * n) due to the memoization table.
  - **Tabulated:** O(m * n) for the DP table.
  - **Space Optimized:** O(n) for storing two rows.

- **Dry Run (Tabulated Approach):**
  Consider the strings `s1 = "horse"` and `s2 = "ros"`. We initialize a DP table of size (6x4), where each cell `(i, j)` represents the minimum number of operations required to convert `s1[0...i]` to `s2[0...j]`. We fill the table starting from the base cases and consider insert, delete, and replace operations for each character pair from both strings. Finally, the result is stored in `dp[0][0]`, which gives the minimum number of operations. For this input, the result is `3`.

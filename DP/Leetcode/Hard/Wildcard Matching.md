### Code:
```cpp
class Solution {
public:
    // Recursive solution
    bool solveRec(string &s, string &p, int i, int j) {
        if (i < 0 && j < 0) return true;
        if (i >= 0 && j < 0) return false;
        if (i < 0 && j >= 0) {
            for (int k = 0; k <= j; k++) {
                if (p[k] != '*') return false;
            }
            return true;
        }
        if (s[i] == p[j] || p[j] == '?') return solveRec(s, p, i - 1, j - 1);
        if (p[j] == '*') return solveRec(s, p, i - 1, j) || solveRec(s, p, i, j - 1);
        return false;
    }

    // Memoized solution
    bool solveMem(string &s, string &p, int i, int j, vector<vector<int>> &dp) {
        if (i < 0 && j < 0) return true;
        if (i >= 0 && j < 0) return false;
        if (i < 0 && j >= 0) {
            for (int k = 0; k <= j; k++) {
                if (p[k] != '*') return false;
            }
            return true;
        }
        if (dp[i][j] != -1) return dp[i][j];
        if (s[i] == p[j] || p[j] == '?') return dp[i][j] = solveMem(s, p, i - 1, j - 1, dp);
        if (p[j] == '*') return dp[i][j] = solveMem(s, p, i - 1, j, dp) || solveMem(s, p, i, j - 1, dp);
        return dp[i][j] = false;
    }

    // Bottom-Up Tabulation
    bool solveTab(string s, string p) {
        int n = s.size(), m = p.size();
        vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));
        dp[0][0] = true;
        for (int j = 1; j <= m; j++) {
            bool flag = true;
            for (int k = 1; k <= j; k++) {
                if (p[k - 1] != '*') {
                    flag = false;
                    break;
                }
            }
            dp[0][j] = flag;
        }
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (s[i - 1] == p[j - 1] || p[j - 1] == '?') dp[i][j] = dp[i - 1][j - 1];
                else if (p[j - 1] == '*') dp[i][j] = dp[i - 1][j] || dp[i][j - 1];
                else dp[i][j] = false;
            }
        }
        return dp[n][m];
    }

    // Space-Optimized Tabulation
    bool solveTabOpt(string s, string p) {
        int n = s.size(), m = p.size();
        vector<int> curr(m + 1, 0), prev(m + 1, 0);
        prev[0] = true;
        for (int j = 1; j <= m; j++) {
            bool flag = true;
            for (int k = 1; k <= j; k++) {
                if (p[k - 1] != '*') {
                    flag = false;
                    break;
                }
            }
            prev[j] = flag;
        }
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (s[i - 1] == p[j - 1] || p[j - 1] == '?') curr[j] = prev[j - 1];
                else if (p[j - 1] == '*') curr[j] = prev[j] || curr[j - 1];
                else curr[j] = false;
            }
            prev = curr;
        }
        return prev[m];
    }

    bool isMatch(string s, string p) {
        return solveTab(s, p);
    }
};
```

### Explanation

- **solveRec**: This recursive function matches `s` and `p` character-by-character:
  - Base cases check for empty strings.
  - If `p[j]` is `*`, we attempt to match it with zero characters or one character recursively.
  - If `p[j]` is `?`, it matches any character in `s[i]`.
- **solveMem**: This memoized function stores previously computed values to optimize recursion, avoiding repeated calculations.
- **solveTab**: This bottom-up tabulation approach iteratively fills a 2D `dp` table where `dp[i][j]` represents if `s[0..i-1]` matches `p[0..j-1]`.
- **solveTabOpt**: This optimized version uses two 1D arrays, `prev` and `curr`, to store only the previous row of results, reducing space complexity.

### Time Complexity

- **solveRec**: \(O(2^{n+m})\), exponential due to recursive branching.
- **solveMem**: \(O(n \times m)\), each state calculated once.
- **solveTab** and **solveTabOpt**: \(O(n \times m)\), both run a nested loop over `n` and `m`.

### Space Complexity

- **solveRec**: \(O(n + m)\), recursion stack.
- **solveMem** and **solveTab**: \(O(n \times m)\), for the `dp` array.
- **solveTabOpt**: \(O(m)\), only two rows are stored.

### Dry Run

For this dry run, we will use the input `s = "aa"` and `p = "*a"`.

1. **Initial Setup**:
   - Initialize a 2D `dp` array with dimensions `(len(s) x len(p))` and all values set to `-1` to indicate uncomputed states.
   - Call `solveMem(s, p, i = 1, j = 1, dp)` where `i` and `j` represent the last indices of `s` and `p` respectively.

2. **Recursive Calls**:
   - At each step, evaluate base cases or recursive transitions based on the characters in `s` and `p`.

#### Step-by-Step Dry Run

1. **Call**: `solveMem(s, p, i = 1, j = 1, dp)`
   - Characters: `s[1] = 'a'`, `p[1] = 'a'`.
   - Since `s[1]` matches `p[1]`, make a recursive call to `solveMem(s, p, i = 0, j = 0, dp)`.

2. **Call**: `solveMem(s, p, i = 0, j = 0, dp)`
   - Characters: `s[0] = 'a'`, `p[0] = '*'`.
   - Since `p[0]` is `*`, attempt two paths:
     - Path 1: Treat `*` as matching zero characters, so call `solveMem(s, p, i = 0, j = -1, dp)`.
     - Path 2: Treat `*` as matching one character, so call `solveMem(s, p, i = -1, j = 0, dp)`.

3. **Call**: `solveMem(s, p, i = 0, j = -1, dp)`
   - Base Case: `i >= 0` and `j < 0`, so return `false` (pattern is exhausted but string is not).

4. **Call**: `solveMem(s, p, i = -1, j = 0, dp)`
   - Base Case: `i < 0` and `j >= 0`.
   - Check if `p[0]` can match an empty string. Since `p[0]` is `*`, it can represent an empty sequence, so return `true`.

5. **Return to Call**: `solveMem(s, p, i = 0, j = 0, dp)`
   - Path 1 returned `false`, and Path 2 returned `true`.
   - Result: `dp[0][0] = true`.

6. **Return to Call**: `solveMem(s, p, i = 1, j = 1, dp)`
   - Recursive call `solveMem(s, p, i = 0, j = 0, dp)` returned `true`.
   - Result: `dp[1][1] = true`.

### Final Result

After completing the recursive calls and storing results in `dp`, the final result is `dp[1][1] = true`, indicating that `s = "aa"` matches `p = "*a"`.

### Code:
```cpp
class Solution {
public:
    // Recursive function
    bool solveRec(string &s, string &p, int i, int j) {
        if(i < 0 && j < 0) return true;
        if(i >= 0 && j < 0) return false;
        if(i < 0 && j >= 0) {
            for(int k = 0; k <= j; k++) {
                if(p[k] != '*') return false;
            }
            return true;
        }
        if(s[i] == p[j] || p[j] == '?') return solveRec(s, p, i - 1, j - 1);
        if(p[j] == '*') return solveRec(s, p, i - 1, j) || solveRec(s, p, i, j - 1);
        return false;
    }
    
    // Memoized function
    bool solveMem(string &s, string &p, int i, int j, vector<vector<int>> &dp) {
        if(i < 0 && j < 0) return true;
        if(i >= 0 && j < 0) return false;
        if(i < 0 && j >= 0) {
            for(int k = 0; k <= j; k++) {
                if(p[k] != '*') return false;
            }
            return true;
        }
        if(dp[i][j] != -1) return dp[i][j];
        
        if(s[i] == p[j] || p[j] == '?') 
            return dp[i][j] = solveMem(s, p, i - 1, j - 1, dp);
        if(p[j] == '*') 
            return dp[i][j] = solveMem(s, p, i - 1, j, dp) || solveMem(s, p, i, j - 1, dp);
        return dp[i][j] = false;
    }
    
    // Tabulation function
    bool solveTab(string s, string p) {
        int n = s.size();
        int m = p.size();
        vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));
        dp[0][0] = true;
        
        for(int j = 1; j <= m; j++) {
            dp[0][j] = (p[j - 1] == '*' && dp[0][j - 1]);
        }
        
        for(int i = 1; i <= n; i++) {
            for(int j = 1; j <= m; j++) {
                if(s[i - 1] == p[j - 1] || p[j - 1] == '?') 
                    dp[i][j] = dp[i - 1][j - 1];
                else if(p[j - 1] == '*') 
                    dp[i][j] = dp[i - 1][j] || dp[i][j - 1];
            }
        }
        return dp[n][m];
    }
    
    // Space-Optimized Tabulation function
    bool solveTabOpt(string s, string p) {
        int n = s.size();
        int m = p.size();
        vector<int> prev(m + 1, 0), curr(m + 1, 0);
        prev[0] = true;
        
        for(int j = 1; j <= m; j++) {
            prev[j] = (p[j - 1] == '*' && prev[j - 1]);
        }
        
        for(int i = 1; i <= n; i++) {
            for(int j = 1; j <= m; j++) {
                if(s[i - 1] == p[j - 1] || p[j - 1] == '?') 
                    curr[j] = prev[j - 1];
                else if(p[j - 1] == '*') 
                    curr[j] = prev[j] || curr[j - 1];
                else 
                    curr[j] = false;
            }
            prev = curr;
        }
        return prev[m];
    }
    
    // Wrapper function
    int wildCard(string pattern, string str) {
        // Use any of the four functions for the solution
        // Memoization method
        // vector<vector<int>> dp(str.size(), vector<int>(pattern.size(), -1));
        // return solveMem(str, pattern, str.size() - 1, pattern.size() - 1, dp) ? 1 : 0;

        // Tabulation method
        return solveTab(str, pattern);
        
        // Or, use the space-optimized tabulation method
        // return solveTabOpt(str, pattern) ? 1 : 0;
    }
};
```

### Explanation
The goal is to determine if the string `s` matches the pattern `p`, which includes two special characters: 
- `?` which matches any single character.
- `*` which matches any sequence of characters (including an empty sequence).

The solution includes multiple approaches:
1. **Recursive Approach**: Solves by recursive calls, checking each character in `s` and `p` and handling wildcard cases.
2. **Memoization Approach**: Stores intermediate results to avoid recomputation.
3. **Tabulation Approach**: Builds a 2D `dp` table iteratively to track matches for substrings of `s` and `p`.
4. **Space-Optimized Tabulation**: Uses two 1D arrays, `prev` and `curr`, to reduce space complexity.

### Time Complexity
- **Recursive**: \(O(2^{n+m})\) due to repeated recursive calls.
- **Memoization**: \(O(n \times m)\), where \(n\) and \(m\) are the lengths of `s` and `p`.
- **Tabulation**: \(O(n \times m)\) since we compute values for each cell in the `dp` table.
- **Space-Optimized Tabulation**: \(O(n \times m)\) with reduced space complexity of \(O(m)\).

### Space Complexity
- **Recursive**: \(O(n + m)\) for the recursion stack.
- **Memoization**: \(O(n \times m)\) for the `dp` table.
- **Tabulation**: \(O(n \times m)\) for the `dp` table.
- **Space-Optimized Tabulation**: \(O(m)\) for `prev` and `curr` arrays.

### Dry Run (Memoization Example)
For `str = "aa"` and `pattern = "*a"`:
1. Initialize `dp` table with all values as `-1`.
2. Begin with `solveMem(str, pattern, 1, 1, dp)`.
3. Each character in `str` and `pattern` is evaluated based on `*` and `?` rules.
4. Intermediate results are stored in `dp` to avoid redundant calculations.
5. The final result is obtained from `dp[n-1][m-1]`, indicating if the pattern matches the entire string.

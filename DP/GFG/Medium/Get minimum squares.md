
### Code
```cpp
class Solution {
public:
    // Recursive approach
    int recur(int n) {
        if (n == 0) return 0;
        int ans = INT_MAX;
        for (int i = 1; i * i <= n; i++) {
            ans = min(ans, recur(n - i * i) + 1);
        }
        return ans;
    }

    // Memoization approach
    int memoHelper(int n, vector<int>& dp) {
        if (n == 0) return 0;
        if (dp[n] != -1) return dp[n];
        int ans = INT_MAX;
        for (int i = 1; i * i <= n; i++) {
            ans = min(ans, memoHelper(n - i * i, dp) + 1);
        }
        return dp[n] = ans;
    }

    int memo(int n) {
        vector<int> dp(n + 1, -1);
        return memoHelper(n, dp);
    }

    // Tabulation approach
    int tabu(int n) {
        vector<int> dp(n + 1, INT_MAX);
        dp[0] = 0;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j * j <= i; j++) {
                dp[i] = min(dp[i], dp[i - j * j] + 1);
            }
        }
        return dp[n];
    }

    int numSquares(int n) {
        // Uncomment the desired approach to call
        // return recur(n);    // Recursive approach
        // return memo(n);     // Memoization approach
        return tabu(n);        // Tabulation approach
    }
};
```

### Explanation
- **Recursive Approach (`recur`)**: This function calculates the least number of perfect square numbers that sum to `n` using recursion. It checks all perfect squares less than or equal to `n`, making a recursive call for each perfect square.
  
- **Memoization Approach (`memo` and `memoHelper`)**: This technique optimizes the recursive solution by storing results of previously computed states in a `dp` array to avoid redundant calculations. The `memoHelper` function checks if the result for `n` is already computed; if not, it computes it recursively.

- **Tabulation Approach (`tabu`)**: This bottom-up dynamic programming technique iteratively fills the `dp` array for all values from `1` to `n`. It checks each possible perfect square for each number up to `n` and updates the `dp` array with the minimum counts.

### Time Complexity
- **Recursive Approach**: \(O(2^{\sqrt{n}})\) in the worst case due to the branching factor, making it inefficient for large `n`.
- **Memoization Approach**: \(O(n \sqrt{n})\) since each value from `1` to `n` is computed at most once, and for each, it checks up to \(\sqrt{n}\) perfect squares.
- **Tabulation Approach**: \(O(n \sqrt{n})\) as it computes the minimum number of squares for every integer from `1` to `n` using previously computed results.

### Space Complexity
- **Recursive Approach**: \(O(n)\) due to the call stack during recursion.
- **Memoization Approach**: \(O(n)\) for the `dp` array used to store results and \(O(n)\) for the call stack, resulting in \(O(n)\) overall.
- **Tabulation Approach**: \(O(n)\) for the `dp` array used to store results.

### Dry Run
Let's do a dry run for `n = 12` using the Tabulation approach.

1. **Initialization**: `dp = [0, INT_MAX, INT_MAX, ..., INT_MAX]` (length `13`)
2. **Outer Loop**: Iterate through `i` from `1` to `12`:
   - **i = 1**:
     - **j = 1**: `dp[1] = min(INT_MAX, dp[0] + 1) = 1`
   - **i = 2**:
     - **j = 1**: `dp[2] = min(INT_MAX, dp[1] + 1) = 2`
   - **i = 3**:
     - **j = 1**: `dp[3] = min(INT_MAX, dp[2] + 1) = 3`
   - **i = 4**:
     - **j = 1**: `dp[4] = min(INT_MAX, dp[3] + 1) = 4`
     - **j = 2**: `dp[4] = min(4, dp[0] + 1) = 1`
   - **i = 5**:
     - **j = 1**: `dp[5] = min(INT_MAX, dp[4] + 1) = 2`
     - **j = 2**: `dp[5] = min(2, dp[1] + 1) = 2`
   - **i = 6**:
     - **j = 1**: `dp[6] = min(INT_MAX, dp[5] + 1) = 3`
     - **j = 2**: `dp[6] = min(3, dp[2] + 1) = 3`
   - **i = 7**:
     - **j = 1**: `dp[7] = min(INT_MAX, dp[6] + 1) = 4`
     - **j = 2**: `dp[7] = min(4, dp[3] + 1) = 4`
   - **i = 8**:
     - **j = 1**: `dp[8] = min(INT_MAX, dp[7] + 1) = 5`
     - **j = 2**: `dp[8] = min(5, dp[4] + 1) = 2`
   - **i = 9**:
     - **j = 1**: `dp[9] = min(INT_MAX, dp[8] + 1) = 3`
     - **j = 2**: `dp[9] = min(3, dp[5] + 1) = 3`
     - **j = 3**: `dp[9] = min(3, dp[0] + 1) = 1`
   - **i = 10**:
     - **j = 1**: `dp[10] = min(INT_MAX, dp[9] + 1) = 2`
     - **j = 2**: `dp[10] = min(2, dp[6] + 1) = 2`
     - **j = 3**: `dp[10] = min(2, dp[1] + 1) = 2`
   - **i = 11**:
     - **j = 1**: `dp[11] = min(INT_MAX, dp[10] + 1) = 3`
     - **j = 2**: `dp[11] = min(3, dp[7] + 1) = 3`
     - **j = 3**: `dp[11] = min(3, dp[2] + 1) = 3`
   - **i = 12**:
     - **j = 1**: `dp[12] = min(INT_MAX, dp[11] + 1) = 4`
     - **j = 2**: `dp[12] = min(4, dp[8] + 1) = 3`
     - **j = 3**: `dp[12] = min(3, dp[3] + 1) = 3`
  
Final `dp` array for `n = 12`: 
`dp = [0, 1, 2, 3, 1, 2, 3, 4, 2, 1, 2, 3, 3]`

The result for `n = 12` is `dp[12] = 3`, indicating that the least number of perfect squares that sum to `12` is `3` (for example, \(4 + 4 + 4\)).

```cpp
class Solution {
public:
    // Recursive function (solveRec)
    int solveRec(string &text1, string &text2, int i, int j) {
        if (i==text1.size() || j==text2.size()) return 0;
        
        if (text1[i]==text2[j])
            return 1+solveRec(text1, text2, i+1, j+1);
        else
            return max(solveRec(text1, text2, i+1, j), solveRec(text1, text2, i, j+1));
    }
    
    // Memoization function (solveMem)
    int solveMem(string &text1, string &text2, int i, int j, vector<vector<int>> &memo) {
        if (i==text1.size() || j==text2.size()) return 0;
        if (memo[i][j]!=-1) return memo[i][j];
        
        if (text1[i]==text2[j])
            memo[i][j]=1+solveMem(text1, text2, i+1, j+1, memo);
        else
            memo[i][j] = max(solveMem(text1, text2, i + 1, j, memo), solveMem(text1, text2, i, j + 1, memo));
        
        return memo[i][j];
    }
    
    // Tabulation function (solveTab)
    int solveTab(string &text1, string &text2) {
        int m = text1.size(), n = text2.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        
        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                if (text1[i] == text2[j])
                    dp[i][j] = 1 + dp[i + 1][j + 1];
                else
                    dp[i][j] = max(dp[i + 1][j], dp[i][j + 1]);
            }
        }
        
        return dp[0][0];
    }
    
    // Space-Optimized function (solveSO)
    int solveSO(string &text1, string &text2) {
        int m=text1.size(), n = text2.size();
        vector<int> curr(n + 1, 0), next(n + 1, 0);
        
        for (int i=m-1; i>=0; i--) {
            for (int j=n-1; j>=0; j--) {
                if (text1[i]==text2[j]) curr[j]=1+next[j+1];
                else curr[j]=max(next[j], curr[j + 1]);
            }
            swap(curr, next);
        }
        
        return next[0];
    }
    
    int longestCommonSubsequence(string text1, string text2) {
        // Recursive call
        // return solveRec(text1, text2, 0, 0);

        // Memoization call
        // vector<vector<int>> memo(text1.size(), vector<int>(text2.size(), -1));
        // return solveMem(text1, text2, 0, 0, memo);

        // Tabulation call
        return solveTab(text1, text2);

        // Space-Optimized call
        // return solveSO(text1, text2);
    }
};

```

### Explanation

- **`solveRec`**: Recursively checks for the longest common subsequence. If `text1[i] == text2[j]`, it includes the characters in the LCS and moves both indices forward. Otherwise, it tries skipping either `i` or `j` and takes the maximum result.
  
- **`solveMem`**: Same as `solveRec`, but it stores results in `memo` to avoid redundant calculations.

- **`solveTab`**: Builds a DP table `dp` from the bottom-up, where `dp[i][j]` stores the LCS length of `text1[i:]` and `text2[j:]`.

- **`solveSO`**: Optimizes space by only keeping two rows `curr` and `next`, simulating the DP table to reduce memory usage from O(m*n) to O(n).

### Complexity Analysis

- **Time Complexity**:
  - `solveRec`: \(O(2^{\min(m, n)})\)
  - `solveMem`: \(O(m \times n)\)
  - `solveTab`: \(O(m \times n)\)
  - `solveSO`: \(O(m \times n)\)
  
- **Space Complexity**:
  - `solveRec`: \(O(\min(m, n))\) for recursion stack.
  - `solveMem`: \(O(m \times n)\) for `memo`.
  - `solveTab`: \(O(m \times n)\) for `dp`.
  - `solveSO`: \(O(n)\)


# Dry Run
Let's go through a dry run of the tabulation approach (`solveTab`) for the example input:

```cpp
text1 = "abcde", text2 = "ace"
```

### Step-by-Step Dry Run with Tabulation

#### Initial Setup
1. Let `m` = length of `text1` = 5, and `n` = length of `text2` = 3.
2. We create a 2D DP array `dp` of size `(m + 1) x (n + 1)`, initialized to 0.
   - `dp[i][j]` will store the longest common subsequence (LCS) length between `text1[i:]` and `text2[j:]`.

#### Fill the DP Table from Bottom to Top
We'll iterate from `i = m - 1` to `0` and from `j = n - 1` to `0`.

**DP Array Initialization**:
   ```
   text1 = "abcde", text2 = "ace"
   m = 5, n = 3
   dp = 
   [[0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 0, 0, 0]]
   ```

#### Iterations:

1. **i = 4, j = 2**:
   - Compare `text1[4]` (`'e'`) with `text2[2]` (`'e'`).
   - They match, so `dp[4][2] = 1 + dp[5][3] = 1`.
   ```
   dp = 
   [[0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 0, 1, 0],
    [0, 0, 0, 0]]
   ```

2. **i = 4, j = 1**:
   - Compare `text1[4]` (`'e'`) with `text2[1]` (`'c'`).
   - They don't match, so `dp[4][1] = max(dp[5][1], dp[4][2]) = max(0, 1) = 1`.
   ```
   dp = 
   [[0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 1, 1, 0],
    [0, 0, 0, 0]]
   ```

3. **i = 4, j = 0**:
   - Compare `text1[4]` (`'e'`) with `text2[0]` (`'a'`).
   - They don't match, so `dp[4][0] = max(dp[5][0], dp[4][1]) = max(0, 1) = 1`.
   ```
   dp = 
   [[0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 0, 0, 0],
    [1, 1, 1, 0],
    [0, 0, 0, 0]]
   ```

4. **i = 3, j = 2**:
   - Compare `text1[3]` (`'d'`) with `text2[2]` (`'e'`).
   - They don't match, so `dp[3][2] = max(dp[4][2], dp[3][3]) = max(1, 0) = 1`.
   ```
   dp = 
   [[0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 0, 1, 0],
    [1, 1, 1, 0],
    [0, 0, 0, 0]]
   ```

5. **i = 3, j = 1**:
   - Compare `text1[3]` (`'d'`) with `text2[1]` (`'c'`).
   - They don't match, so `dp[3][1] = max(dp[4][1], dp[3][2]) = max(1, 1) = 1`.
   ```
   dp = 
   [[0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 1, 1, 0],
    [1, 1, 1, 0],
    [0, 0, 0, 0]]
   ```

6. **i = 3, j = 0**:
   - Compare `text1[3]` (`'d'`) with `text2[0]` (`'a'`).
   - They don't match, so `dp[3][0] = max(dp[4][0], dp[3][1]) = max(1, 1) = 1`.
   ```
   dp = 
   [[0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 0, 0, 0],
    [1, 1, 1, 0],
    [1, 1, 1, 0],
    [0, 0, 0, 0]]
   ```

7. **i = 2, j = 2**:
   - Compare `text1[2]` (`'c'`) with `text2[2]` (`'e'`).
   - They don't match, so `dp[2][2] = max(dp[3][2], dp[2][3]) = max(1, 0) = 1`.
   ```
   dp = 
   [[0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 0, 1, 0],
    [1, 1, 1, 0],
    [1, 1, 1, 0],
    [0, 0, 0, 0]]
   ```

8. **i = 2, j = 1**:
   - Compare `text1[2]` (`'c'`) with `text2[1]` (`'c'`).
   - They match, so `dp[2][1] = 1 + dp[3][2] = 1 + 1 = 2`.
   ```
   dp = 
   [[0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 2, 1, 0],
    [1, 1, 1, 0],
    [1, 1, 1, 0],
    [0, 0, 0, 0]]
   ```

9. **i = 2, j = 0**:
   - Compare `text1[2]` (`'c'`) with `text2[0]` (`'a'`).
   - They don't match, so `dp[2][0] = max(dp[3][0], dp[2][1]) = max(1, 2) = 2`.
   ```
   dp = 
   [[0, 0, 0, 0],
    [0, 0, 0, 0],
    [2, 2, 1, 0],
    [1, 1, 1, 0],
    [1, 1, 1, 0],
    [0, 0, 0, 0]]
   ```

10. **i = 1, j = 2**:
    - Compare `text1[1]` (`'b'`) with `text2[2]` (`'e'`).
    - They don't match, so `dp[1][2] = max(dp[2][2], dp[1][3]) = max(1, 0) = 1`.
    ```
    dp = 
    [[0, 0, 0, 0],
     [0, 0, 1, 0],
     [2, 2, 1, 0],
     [1, 1, 1, 0],
     [1, 1, 1, 0],
     [0, 0, 0, 0]]
    ```

11. **i = 1, j = 1**:
    - Compare `text1[1]` (`'b'`) with `text2[1]` (`'c'`).
    - They don't match, so `dp[1][1] = max(dp[2][1], dp[1][2]) = max(2, 1) = 2`.
    ```
    dp = 
    [[0, 0, 0, 0],
     [0, 2, 1, 0],
     [2, 2, 1, 0],
     [1, 1, 1, 0],
     [1, 1, 1, 0],
     [0, 0, 0, 0]]
    ```

12. **i = 1, j = 0**:
    - Compare `text1[1]` (`'b'`) with `text2[0]` (`'a'`).
    - They don't match, so `dp[1][0] = max(dp[2][0], dp[1][1]) = max(2, 2) = 2`.
    ```
    dp = 
    [[0, 0, 0, 0],
     [2, 2, 1, 0],
     [2, 2, 1, 0],
     [1, 1, 1, 0],
     [1, 1, 1, 0],
     [0, 0, 0, 0]]
    ```

13. **i = 0, j = 2**:
    - Compare `text1[0]` (`'a'`) with `text2[2]` (`'e'`).
    - They don't match, so `dp[0][2] = max(dp[1][2], dp[0][3]) = max(1, 0) = 1`.
    ```
    dp = 
    [[0, 0, 1, 0],
     [2, 2, 1, 0],
     [2, 2, 1, 0],
     [1, 1, 1, 0],
     [1, 1, 1, 0],
     [0, 0, 0, 0]]
    ```

14. **i = 0, j = 1**:
    - Compare `text1[0]` (`'a'`) with `text2[1]` (`'c'`).
    - They don't match, so `dp[0][1] = max(dp[1][1], dp[0][2]) = max(2, 1) = 2`.
    ```
    dp = 
    [[0, 2, 1, 0],
     [2, 2, 1, 0],
     [2, 2, 1, 0],
     [1, 1, 1, 0],
     [1, 1, 1, 0],
     [0, 0, 0, 0]]
    ```

15. **i = 0, j = 0**:
    - Compare `text1[0]` (`'a'`) with `text2[0]` (`'a'`).
    - They match, so `dp[0][0] = 1 + dp[1][1] = 1 + 2 = 3`.
    ```
    dp = 
    [[3, 2, 1, 0],
     [2, 2, 1, 0],
     [2, 2, 1, 0],
     [1, 1, 1, 0],
     [1, 1, 1, 0],
     [0, 0, 0, 0]]
    ```

### Final Result
The length of the longest common subsequence is stored in `dp[0][0]`, which is **3**. The LCS for `"abcde"` and `"ace"` is `"ace"` with a length of 3.

### Final DP Table:

```
[[3, 2, 1, 0],
 [2, 2, 1, 0],
 [2, 2, 1, 0],
 [1, 1, 1, 0],
 [1, 1, 1, 0],
 [0, 0, 0, 0]]
```

This table shows the LCS length at each position `(i, j)`, which helps us derive the result from `dp[0][0]`.

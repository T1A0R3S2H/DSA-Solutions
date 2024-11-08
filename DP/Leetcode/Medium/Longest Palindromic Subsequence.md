### Code
```cpp
class Solution {
public:
    // Recursive function (solveRec)
    int solveRec(string &text1, string &text2, int i, int j) {
        if (i==text1.size() || j==text2.size()) return 0;
        if (text1[i]==text2[j]){
            return 1+solveRec(text1, text2, i+1, j+1);
        }
        else{
            return max(solveRec(text1, text2, i+1, j), solveRec(text1, text2, i, j+1));
        }
    }
    
    // Memoization function (solveMem)
    int solveMem(string &text1, string &text2, int i, int j, vector<vector<int>> &memo) {
        if (i==text1.size() || j==text2.size()) return 0;
        if (memo[i][j]!=-1) return memo[i][j];
        if (text1[i]==text2[j]){
            memo[i][j]=1+solveMem(text1, text2, i+1, j+1, memo);
        }
        else{
            memo[i][j] = max(solveMem(text1, text2, i + 1, j, memo), solveMem(text1, text2, i, j + 1, memo));
        }
        return memo[i][j];
    }
    
    // Tabulation function (solveTab)
    int solveTab(string &text1, string &text2) {
        int m=text1.size(), n = text2.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        for (int i= m-1; i>=0; i--) {
            for (int j=n-1; j >= 0; j--) {
                if (text1[i]==text2[j]) dp[i][j]=1+dp[i+1][j+1];
                else dp[i][j]=max(dp[i+1][j], dp[i][j+1]);
            }
        }
        return dp[0][0];
    }
    
    // Space-Optimized function (solveSO)
    int solveSO(string &text1, string &text2) {
        int m=text1.size(), n = text2.size();
        vector<int> curr(n+1, 0), next(n + 1, 0);
        for (int i=m-1; i>=0; i--) {
            for (int j=n-1; j>=0; j--) {
                if (text1[i]==text2[j]) curr[j]=1+next[j+1];
                else curr[j]=max(next[j], curr[j + 1]);
            }
            swap(curr, next);
        }
        return next[0];
    }

    int longestPalindromeSubseq(string s) {
        string revS=s;
        reverse(revS.begin(), revS.end());
        int ans=solveTab(s, revS);
        return ans;

        // Recursive call
        // return solveRec(text1, text2, 0, 0);

        // Memoization call
        // vector<vector<int>> memo(text1.size(), vector<int>(text2.size(), -1));
        // return solveMem(text1, text2, 0, 0, memo);

        // Tabulation call
        // return solveTab(text1, text2);

        // Space-Optimized call
        // return solveSO(text1, text2);
    }
};
```

### Explanation

1. **Problem Approach**: The longest palindromic subsequence problem can be transformed into finding the longest common subsequence (LCS) between the input string `s` and its reverse. This transformation works because the longest palindromic subsequence in `s` will also appear as the longest common subsequence between `s` and its reverse.
  
2. **Recursive Function (`solveRec`)**:
   - `solveRec` compares characters in `text1` and `text2` starting from indices `i` and `j`. If the characters match, it adds `1` to the result and moves both indices forward. If they don't match, it considers two cases: incrementing `i` or `j` and returning the maximum result from those two calls.
  
3. **Memoization Function (`solveMem`)**:
   - `solveMem` is an optimized version of `solveRec` that uses a 2D `memo` array to store results of overlapping subproblems, reducing repeated computations.
  
4. **Tabulation (`solveTab`)**:
   - This function builds a bottom-up 2D `dp` table where `dp[i][j]` holds the LCS length for `text1[i:]` and `text2[j:]`.
  
5. **Space Optimization (`solveSO`)**:
   - `solveSO` uses two 1D arrays (`curr` and `next`) to save memory while computing LCS, as only the previous row is needed for computation.
  
6. **Main Function (`longestPalindromeSubseq`)**:
   - This function reverses the input string `s` to create `revS` and calls `solveTab` with `s` and `revS` to get the LCS length, which is also the longest palindromic subsequence length.

### Time Complexity

- **Recursive** (`solveRec`): \(O(2^{m+n})\) due to exponential recursive calls.
- **Memoization** (`solveMem`): \(O(m \times n)\), where `m` and `n` are the lengths of `text1` and `text2`, as each state `(i, j)` is computed only once.
- **Tabulation** (`solveTab`): \(O(m \times n)\), as it fills up a `dp` table of size `m x n`.
- **Space-Optimized** (`solveSO`): \(O(m \times n)\), as it iterates over a 1D array of size `n` for each of the `m` rows.

### Space Complexity

- **Recursive** (`solveRec`): \(O(m + n)\), due to recursive call stack depth.
- **Memoization** (`solveMem`): \(O(m \times n)\) for the `memo` table.
- **Tabulation** (`solveTab`): \(O(m \times n)\) for the `dp` table.
- **Space-Optimized** (`solveSO`): \(O(n)\), as it only uses two 1D arrays of length `n`.

### TLDR
The Longest Common Subsequence (LCS) approach works for the Longest Palindromic Subsequence (LPS) problem because a palindrome reads the same forwards and backwards, so any palindromic subsequence in the original string `s` will also be present in the reverse of `s` (`revS`). By finding the LCS of `s` and `revS`, we are effectively identifying the longest sequence of characters that appear in the same order in both `s` and `revS`, which corresponds to the longest sequence of characters that form a palindrome in `s`. Therefore, the LCS of `s` and `revS` gives the length of the longest palindromic subsequence in `s`. This approach leverages the symmetry of palindromes without explicitly searching for palindromic patterns, making it efficient for LPS.

### Dry Run

Let's go through a detailed dry run of the **Tabulation approach (`solveTab`)** for the input `s = "bbbab"`.

1. **Initial Setup**:
   - Input: `s = "bbbab"`
   - Reverse `s` to get `revS = "babbb"`
   - Define `m` and `n` as the lengths of `s` and `revS`, which are both `5`.
   - Initialize a `dp` table of size `6 x 6` (to handle base cases with zero-length prefixes) filled with `0`s.

2. **Goal**:
   - We need to fill `dp[i][j]`, where `dp[i][j]` represents the longest common subsequence length for `s[i:]` and `revS[j:]`.

3. **DP Table Filling (Bottom-Up)**:
   - We fill the `dp` table from the bottom-right to the top-left, iterating over indices `i` from `m-1` to `0` and `j` from `n-1` to `0`.
   - If `s[i] == revS[j]`, then `dp[i][j] = 1 + dp[i+1][j+1]`.
   - Otherwise, `dp[i][j] = max(dp[i+1][j], dp[i][j+1])`.

4. **Step-by-Step Filling of the DP Table**:

   Let's go through each iteration, filling the table based on character matches between `s` and `revS`.

   | i | j | `s[i]` | `revS[j]` | `dp[i][j]` Calculation                         | `dp[i][j]` Value |
   |---|---|---------|-----------|------------------------------------------------|-------------------|
   | 4 | 4 | b       | b         | `dp[4][4] = 1 + dp[5][5]` (match)             | 1                |
   | 4 | 3 | b       | b         | `dp[4][3] = 1 + dp[5][4]` (match)             | 1                |
   | 4 | 2 | b       | b         | `dp[4][2] = 1 + dp[5][3]` (match)             | 1                |
   | 4 | 1 | b       | a         | `dp[4][1] = max(dp[5][1], dp[4][2])`          | 1                |
   | 4 | 0 | b       | b         | `dp[4][0] = 1 + dp[5][1]` (match)             | 1                |
   | 3 | 4 | a       | b         | `dp[3][4] = max(dp[4][4], dp[3][5])`          | 1                |
   | 3 | 3 | a       | b         | `dp[3][3] = max(dp[4][3], dp[3][4])`          | 1                |
   | 3 | 2 | a       | b         | `dp[3][2] = max(dp[4][2], dp[3][3])`          | 1                |
   | 3 | 1 | a       | a         | `dp[3][1] = 1 + dp[4][2]` (match)             | 2                |
   | 3 | 0 | a       | b         | `dp[3][0] = max(dp[4][0], dp[3][1])`          | 2                |
   | 2 | 4 | b       | b         | `dp[2][4] = 1 + dp[3][5]` (match)             | 1                |
   | 2 | 3 | b       | b         | `dp[2][3] = 1 + dp[3][4]` (match)             | 2                |
   | 2 | 2 | b       | b         | `dp[2][2] = 1 + dp[3][3]` (match)             | 2                |
   | 2 | 1 | b       | a         | `dp[2][1] = max(dp[3][1], dp[2][2])`          | 2                |
   | 2 | 0 | b       | b         | `dp[2][0] = 1 + dp[3][1]` (match)             | 3                |
   | 1 | 4 | b       | b         | `dp[1][4] = 1 + dp[2][5]` (match)             | 1                |
   | 1 | 3 | b       | b         | `dp[1][3] = 1 + dp[2][4]` (match)             | 2                |
   | 1 | 2 | b       | b         | `dp[1][2] = 1 + dp[2][3]` (match)             | 3                |
   | 1 | 1 | b       | a         | `dp[1][1] = max(dp[2][1], dp[1][2])`          | 3                |
   | 1 | 0 | b       | b         | `dp[1][0] = 1 + dp[2][1]` (match)             | 3                |
   | 0 | 4 | b       | b         | `dp[0][4] = 1 + dp[1][5]` (match)             | 1                |
   | 0 | 3 | b       | b         | `dp[0][3] = 1 + dp[1][4]` (match)             | 2                |
   | 0 | 2 | b       | b         | `dp[0][2] = 1 + dp[1][3]` (match)             | 3                |
   | 0 | 1 | b       | a         | `dp[0][1] = max(dp[1][1], dp[0][2])`          | 3                |
   | 0 | 0 | b       | b         | `dp[0][0] = 1 + dp[1][1]` (match)             | 4                |

5. **Final Result**:
   - `dp[0][0]` now holds the length of the longest palindromic subsequence, which is `4` (the subsequence `"bbbb"`).

---

This dry run illustrates how each entry in the `dp` table is computed by comparing characters in `s` and `revS`, updating `dp[i][j]` based on whether the characters match or not, and eventually calculating the longest palindromic subsequence length.

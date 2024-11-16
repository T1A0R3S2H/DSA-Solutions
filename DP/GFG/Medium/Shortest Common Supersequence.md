### Gist:
To find the **Shortest Common Supersequence (SCS)**, we first calculate the **Longest Common Subsequence (LCS)** between the two input strings. The LCS represents the longest sequence that appears in both strings in the same order. Once we have the LCS, we can calculate the length of the SCS using the formula:
\[
\text{Length of SCS} = \text{Length of str1} + \text{Length of str2} - \text{Length of LCS}
\]
The reason this works is that the characters common to both strings (represented by the LCS) are counted only once in the SCS. This approach allows us to determine the length of the shortest supersequence without explicitly constructing it.

By leveraging **dynamic programming** to compute the LCS and using this formula, we can efficiently compute the length of the SCS.

---
### Code:

```cpp
class Solution {
public:
    int solveMem(string &text1, string &text2, int i, int j, vector<vector<int>> &memo) {
        if (i == text1.size() || j == text2.size()) return 0;
        if (memo[i][j] != -1) return memo[i][j];
        
        if (text1[i] == text2[j])
            memo[i][j] = 1 + solveMem(text1, text2, i + 1, j + 1, memo);
        else
            memo[i][j] = max(solveMem(text1, text2, i + 1, j, memo), solveMem(text1, text2, i, j + 1, memo));
        
        return memo[i][j];
    }

    int shortestCommonSupersequence(string str1, string str2) {
        int m = str1.size(), n = str2.size();
        vector<vector<int>> memo(m, vector<int>(n, -1));
        
        // Step 1: Calculate LCS
        int lcsLength = solveMem(str1, str2, 0, 0, memo);
        
        // Step 2: Calculate SCS length
        return m + n - lcsLength;  // The length of SCS is sum of lengths of str1 and str2 minus LCS length
    }
};
```

### Explanation:

- **Explanation**: 
  The approach to solving the **Shortest Common Supersequence (SCS)** problem involves first calculating the **Longest Common Subsequence (LCS)** of the two strings. The LCS helps to identify the characters that are common between the two strings, which will appear only once in the final SCS. After finding the LCS, the length of the SCS is calculated using the formula:
  \[
  \text{Length of SCS} = \text{Length of str1} + \text{Length of str2} - \text{Length of LCS}
  \]
  This formula works because the characters that are common between the two strings are counted only once in the final SCS. The function returns the length of the **SCS** instead of the actual string.

### Time Complexity:
- **O(m * n)**: The time complexity is dominated by the calculation of the **Longest Common Subsequence (LCS)** using dynamic programming, which takes O(m * n) time, where `m` and `n` are the lengths of `str1` and `str2`.

### Space Complexity:
- **O(m * n)**: The space complexity is also O(m * n) due to the memoization table used in the LCS computation.

### Dry Run:

- **Input**: `str1 = "geek"`, `str2 = "eke"`
- **LCS Calculation**: The LCS is `"ek"`, so `lcsLength = 2`.
- **SCS Length Calculation**: The length of the SCS is `4 (len of "geek") + 3 (len of "eke") - 2 (LCS length) = 5`.

Thus, the **output** will be `5`, which matches the expected result.


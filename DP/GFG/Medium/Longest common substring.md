### TL;DR
The problem is to find the **length of the longest common substring** between two strings \(s1\) and \(s2\). A substring is a continuous sequence of characters in a string. The solution uses **dynamic programming** to compute the result efficiently. The main idea is to construct a 2D DP table where each cell represents the length of the common substring ending at specific indices of the two strings. The answer is the maximum value in this table.

---

### CETSD

#### Code
```cpp
class Solution {
public:
    int longestCommonSubstr(string s1, string s2) {
        int n = s1.size(), m = s2.size();
        vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0)); // DP table
        int maxLen = 0; // To track the maximum substring length

        // Populate the DP table
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                if (s1[i] == s2[j]) {
                    dp[i + 1][j + 1] = dp[i][j] + 1; // Extend the substring
                    maxLen = max(maxLen, dp[i + 1][j + 1]); // Update the max length
                } else {
                    dp[i + 1][j + 1] = 0; // Reset if characters don't match
                }
            }
        }
        return maxLen;
    }
};
```

---

#### Explanation
1. **Approach**:
   - Create a DP table `dp` of size \((n+1) \times (m+1)\) where \(dp[i+1][j+1]\) stores the length of the longest common substring ending at \(s1[i]\) and \(s2[j]\).
   - If characters \(s1[i]\) and \(s2[j]\) match, extend the length of the substring from `dp[i][j]`.
   - If they don’t match, reset the current cell to `0` (substring continuity is broken).
   - Track the maximum value in the DP table (`maxLen`).

2. **Initialization**:
   - The first row and column of the DP table are implicitly `0` (no common substring with an empty string).

3. **Result**:
   - The final result is stored in `maxLen`, representing the length of the longest common substring.

---

#### Time Complexity
- **Time Complexity**: \(O(n \times m)\), where \(n = \text{s1.size()}\) and \(m = \text{s2.size()}\).
  - Two nested loops traverse all characters of \(s1\) and \(s2\).
- **Space Complexity**: \(O(n \times m)\) for the DP table.

---

#### Dry Run (Complete)

**Input**:
- \(s1 = "ABCDGH"\)
- \(s2 = "ACDGHR"\)

**DP Table Construction**:

| i\\j |   | A | C | D | G | H | R |
|------|---|---|---|---|---|---|---|
|      | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| A    | 0 | 1 | 0 | 0 | 0 | 0 | 0 |
| B    | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| C    | 0 | 0 | 1 | 0 | 0 | 0 | 0 |
| D    | 0 | 0 | 0 | 2 | 0 | 0 | 0 |
| G    | 0 | 0 | 0 | 0 | 3 | 0 | 0 |
| H    | 0 | 0 | 0 | 0 | 0 | 4 | 0 |

**Step-by-step Construction**:
1. Compare \(s1[0] = A\) with \(s2[0] = A\):
   - Characters match, \(dp[1][1] = dp[0][0] + 1 = 1\).
   - Update `maxLen = 1`.

2. Compare \(s1[1] = B\) with \(s2[0] = A\), \(s2[1] = C\), ...:
   - Characters don’t match; \(dp[2][1] = dp[2][2] = ... = 0\).

3. Compare \(s1[2] = C\) with \(s2[1] = C\):
   - Characters match, \(dp[3][2] = dp[2][1] + 1 = 1\).
   - Update `maxLen = 2`.

4. Repeat the process for all \(i, j\), building the table and tracking the maximum substring length.

**Final Output**:
- \(maxLen = 4\), corresponding to the substring `"CDGH"`.

---

### Summary
- The **Longest Common Substring** is solved using a **2D DP table**.
- The result is the **maximum value in the table**, which represents the length of the longest common substring.
- **Dry Run** validates correctness for inputs \(s1 = "ABCDGH"\), \(s2 = "ACDGHR"\), with a result of \(4\).

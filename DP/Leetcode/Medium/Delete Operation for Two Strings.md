### **Code:**

```cpp
class Solution {
public:
    int solveMem(string &word1, string &word2, int i, int j, vector<vector<int>> &dp) {
        if (i==word1.size()) return word2.size()-j;
        if (j==word2.size()) return word1.size()-i;
        if (dp[i][j]!=-1) return dp[i][j];

        if (word1[i]==word2[j]) {
            return dp[i][j]=solveMem(word1, word2, i+1, j+1, dp);
        } 
        else {
            int deleteWord1=1+solveMem(word1, word2, i+1, j, dp);
            int deleteWord2=1+solveMem(word1, word2, i, j+1, dp); 
            return dp[i][j]=min(deleteWord1, deleteWord2);
        }
    }

    int minDistance(string word1, string word2) {
        int n=word1.size();
        int m=word2.size();
        vector<vector<int>> dp(n, vector<int>(m, -1));
        return solveMem(word1, word2, 0, 0, dp);
    }
};

```

---

### **Explanation:**

1. **Problem Reduction**:
   - The task is to find the minimum deletions needed to make `word1` and `word2` identical.
   - Instead of solving this directly, we focus on finding the **Longest Common Subsequence (LCS)** of the two strings.
   - From the LCS, we calculate:
     \[
     \text{Deletions} = (\text{Length of word1} - \text{LCS}) + (\text{Length of word2} - \text{LCS})
     \]

2. **Recursive Function (`solveMem`)**:
   - We use a helper function to solve the problem recursively with memoization.
   - **Base Cases**:
     - If `i == word1.size()`, return `word2.size() - j`: All remaining characters in `word2` must be deleted.
     - If `j == word2.size()`, return `word1.size() - i`: All remaining characters in `word1` must be deleted.
   - **Recursive Logic**:
     - If `word1[i] == word2[j]`, no deletions are needed for this character; move to the next indices in both strings.
     - Otherwise:
       - Option 1: Delete `word1[i]` and increment `i`.
       - Option 2: Delete `word2[j]` and increment `j`.
       - Take the minimum of the two options, adding 1 for the deletion step.

3. **Dynamic Programming Table (`dp`)**:
   - The `dp[i][j]` stores the minimum deletions required for the substrings `word1[i:]` and `word2[j:]`.
   - This avoids redundant computations by storing previously computed results.

---

### **Time Complexity**:
1. **Recursive Function**:
   - Each subproblem is defined by indices `i` and `j`.
   - There are \(O(n \times m)\) subproblems, where \(n = \text{length of word1}\) and \(m = \text{length of word2}\).
   - Each subproblem is solved in \(O(1)\).
2. **Overall**: \(O(n \times m)\).

---

### **Space Complexity**:
1. **DP Table**: \(O(n \times m)\) for memoization.
2. **Call Stack**: \(O(\min(n, m))\) for recursion depth.
3. **Total**: \(O(n \times m)\).

---

### **Dry Run**:
#### Input:
`word1 = "sea"`, `word2 = "eat"`

#### Step-by-Step Execution:
1. Start with `solveMem("sea", "eat", 0, 0)`:
   - `word1[0] = 's'`, `word2[0] = 'e'` (not equal).
   - Options:
     - Delete `'s'` → \(1 + \text{solveMem("ea", "eat", 1, 0)}\).
     - Delete `'e'` → \(1 + \text{solveMem("sea", "at", 0, 1)}\).

2. Continue recursively:
   - For `solveMem("ea", "eat", 1, 0)`:
     - `word1[1] = 'e'`, `word2[0] = 'e'` (equal).
     - Move to next characters → \(\text{solveMem("a", "at", 2, 1)}\).

   - For `solveMem("a", "at", 2, 1)`:
     - `word1[2] = 'a'`, `word2[1] = 'a'` (equal).
     - Move to next characters → \(\text{solveMem("", "t", 3, 2)}\).

   - For `solveMem("", "t", 3, 2)`:
     - `i == word1.size()`, return `word2.size() - j = 1`.

3. Combine results:
   - Total deletions for this path = \(1 + 0 = 1\).

4. For the other path (`solveMem("sea", "at", 0, 1)`):
   - Similar calculations yield a total deletions of 2.

5. Final Result:
   - Minimum deletions = \(2\).

#### Output:
`2`

---

### **Summary**:
- **Steps**: Calculate the LCS via recursive logic, compute deletions.
- **Efficiency**: \(O(n \times m)\) time and space complexity.
- **Output**: Correct minimum deletions for any input strings.

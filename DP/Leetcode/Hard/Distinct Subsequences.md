### C++ Code (Recursion+Memoisation):
```cpp
class Solution {
public:
    int solve(int i, int j, string &s, string &t, vector<vector<int>> &dp) {
        // If t is completely matched, count this as 1 valid way
        if (j==t.size()) return 1;
        if (i==s.size()) return 0;
        if (dp[i][j]!=-1) return dp[i][j];

        int take=0;
        int notTake=0;
        if (s[i]==t[j]) {
            // If characters match, we can take this character
            take=solve(i+1, j+1, s, t, dp);
        }
        // Skip the current character of s
        notTake=solve(i+1, j, s, t, dp);
        return dp[i][j]=take+notTake;
    }

    int numDistinct(string s, string t) {
        int n=s.size();
        int m=t.size();
        vector<vector<int>> dp(n, vector<int>(m, -1));
        return solve(0, , s, t, dp);
    }
};

```
### TL;DR

The given code calculates the number of distinct subsequences of string `s` that match string `t` using recursion with memoization. It checks all possible ways to include or exclude each character of `s` to form `t`, while storing intermediate results in a `dp` table for efficiency.

---

### ETSD (Explanation, Time Complexity, Space Complexity, Dry Run)

#### **Explanation**

1. **Recursive Approach**:
   - Start at the first character of both strings (`i=0, j=0`).
   - For each character in `s`, decide:
     - **Take**: If `s[i] == t[j]`, include this character and move to the next character in both `s` and `t`.
     - **Not Take**: Skip the current character of `s` and only move to the next character in `s`.

2. **Base Cases**:
   - If `j == t.size()`: All characters of `t` are matched, return `1` (valid subsequence found).
   - If `i == s.size()`: All characters of `s` are processed but `t` is not fully matched, return `0`.

3. **Memoization**:
   - Use a 2D `dp` table (`dp[i][j]`) to store results of subproblems:
     - `dp[i][j]` represents the number of distinct subsequences of `s[i:]` that match `t[j:]`.

4. **Recursive Formula**:
   - If `s[i] == t[j]`:  
     `dp[i][j] = solve(i+1, j+1) + solve(i+1, j)`
   - Otherwise:  
     `dp[i][j] = solve(i+1, j)`

5. **Driver Function (`numDistinct`)**:
   - Initializes the `dp` table with `-1` to denote uncomputed states.
   - Starts the recursive computation from the first characters of `s` and `t`.

---

#### **Time Complexity**

- **Subproblem States**: There are \(n \times m\) unique states in the `dp` table.
- **Transition Complexity**: Each state performs \(O(1)\) operations.
- **Final Complexity**:  
  \[
  O(n \times m)
  \]  
  where \(n = \text{s.size()}\) and \(m = \text{t.size()}\).

---

#### **Space Complexity**

1. **DP Table**: \(O(n \times m)\) for storing results of subproblems.
2. **Recursion Stack**: \(O(n + m)\) for the maximum depth of recursion.
3. **Total Space Complexity**:  
   \[
   O(n \times m)
   \]

---

#### **Dry Run**

**Input**: `s = "babgbag"`, `t = "bag"`

**Initialization**:
- \(n = 7\) (length of \(s\)), \(m = 3\) (length of \(t\)).
- `dp` table initialized to size \(7 \times 3\) with all values as `-1`.

#### **Steps**:

We recursively compute the number of distinct subsequences of `s[i:]` that match `t[j:]`. For clarity, the computations at each step are as follows:

---

### **Step 1: Initial Call**
\[
\text{solve}(i = 0, j = 0)
\]
- Current characters: \(s[0] = 'b'\), \(t[0] = 'b'\).
- Characters match. Two choices:
  - **Take**: Move to \(\text{solve}(i = 1, j = 1)\).
  - **Not Take**: Move to \(\text{solve}(i = 1, j = 0)\).

---

### **Step 2: Recursive Call 1 (Take)**  
\[
\text{solve}(i = 1, j = 1)
\]
- Current characters: \(s[1] = 'a'\), \(t[1] = 'a'\).
- Characters match. Two choices:
  - **Take**: Move to \(\text{solve}(i = 2, j = 2)\).
  - **Not Take**: Move to \(\text{solve}(i = 2, j = 1)\).

---

### **Step 3: Recursive Call 2 (Take from Step 2)**  
\[
\text{solve}(i = 2, j = 2)
\]
- Current characters: \(s[2] = 'b'\), \(t[2] = 'g'\).
- Characters do **not** match. One choice:
  - **Not Take**: Move to \(\text{solve}(i = 3, j = 2)\).

---

### **Step 4: Recursive Call 3 (Not Take from Step 3)**  
\[
\text{solve}(i = 3, j = 2)
\]
- Current characters: \(s[3] = 'g'\), \(t[2] = 'g'\).
- Characters match. Two choices:
  - **Take**: Move to \(\text{solve}(i = 4, j = 3)\).
  - **Not Take**: Move to \(\text{solve}(i = 4, j = 2)\).

---

### **Step 5: Recursive Call 4 (Take from Step 4)**  
\[
\text{solve}(i = 4, j = 3)
\]
- **Base Case**: \(j = 3\) (all characters of \(t\) are matched).
- Return \(1\).

---

### **Step 6: Recursive Call 5 (Not Take from Step 4)**  
\[
\text{solve}(i = 4, j = 2)
\]
- Current characters: \(s[4] = 'b'\), \(t[2] = 'g'\).
- Characters do **not** match. One choice:
  - **Not Take**: Move to \(\text{solve}(i = 5, j = 2)\).

---

### **Step 7: Recursive Call 6 (Not Take from Step 6)**  
\[
\text{solve}(i = 5, j = 2)
\]
- Current characters: \(s[5] = 'a'\), \(t[2] = 'g'\).
- Characters do **not** match. One choice:
  - **Not Take**: Move to \(\text{solve}(i = 6, j = 2)\).

---

### **Step 8: Recursive Call 7 (Not Take from Step 7)**  
\[
\text{solve}(i = 6, j = 2)
\]
- Current characters: \(s[6] = 'g'\), \(t[2] = 'g'\).
- Characters match. Two choices:
  - **Take**: Move to \(\text{solve}(i = 7, j = 3)\).
  - **Not Take**: Move to \(\text{solve}(i = 7, j = 2)\).

---

### **Step 9: Recursive Call 8 (Take from Step 8)**  
\[
\text{solve}(i = 7, j = 3)
\]
- **Base Case**: \(j = 3\) (all characters of \(t\) are matched).
- Return \(1\).

---

### **Step 10: Recursive Call 9 (Not Take from Step 8)**  
\[
\text{solve}(i = 7, j = 2)
\]
- **Base Case**: \(i = 7\) (all characters of \(s\) are processed).
- Return \(0\).

---

### **Backtracking and Aggregating Results**

#### Step 8:
\[
\text{solve}(i = 6, j = 2) = 1 (\text{Take}) + 0 (\text{Not Take}) = 1
\]

#### Step 7:
\[
\text{solve}(i = 5, j = 2) = 1 (\text{Result from Step 8})
\]

#### Step 6:
\[
\text{solve}(i = 4, j = 2) = 1 (\text{Result from Step 7})
\]

#### Step 4:
\[
\text{solve}(i = 3, j = 2) = 1 (\text{Take}) + 1 (\text{Not Take}) = 2
\]

#### Step 3:
\[
\text{solve}(i = 2, j = 2) = 2 (\text{Result from Step 4})
\]

#### Step 2:
\[
\text{solve}(i = 1, j = 1) = 2 (\text{Take}) + 0 (\text{Not Take}) = 2
\]

#### Step 1:
\[
\text{solve}(i = 0, j = 0) = 3 (\text{Final Aggregation})
\]

---

### **Final Result**
The number of distinct subsequences of \(s = "babgbag"\) that match \(t = "bag"\) is **5**.

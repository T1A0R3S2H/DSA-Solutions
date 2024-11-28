## **Code**

```cpp
class Solution {
private:
    vector<vector<int>> dp; // DP table to store LCS lengths
    map<pair<int, int>, set<string>> memo; // Memoization for backtracking

    set<string> backtrack(int i, int j, string& s, string& t) {
        if (i == 0 || j == 0) return {""}; // Base case: empty LCS when any string is fully processed

        if (memo.count({i, j})) return memo[{i, j}]; // Return cached result if already computed

        set<string> result;

        if (s[i - 1] == t[j - 1]) { // If characters match, add them to all LCS from (i-1, j-1)
            set<string> temp = backtrack(i - 1, j - 1, s, t);
            for (const string& str : temp) {
                result.insert(str + s[i - 1]);
            }
        } else { // Characters don't match
            if (dp[i - 1][j] == dp[i][j]) { // Explore (i-1, j) if it has the same LCS length
                set<string> temp = backtrack(i - 1, j, s, t);
                result.insert(temp.begin(), temp.end());
            }
            if (dp[i][j - 1] == dp[i][j]) { // Explore (i, j-1) if it has the same LCS length
                set<string> temp = backtrack(i, j - 1, s, t);
                result.insert(temp.begin(), temp.end());
            }
        }

        return memo[{i, j}] = result; // Cache and return the result for (i, j)
    }

public:
    vector<string> all_longest_common_subsequences(string s, string t) {
        int n = s.size(), m = t.size();
        dp = vector<vector<int>>(n + 1, vector<int>(m + 1, 0));

        // Step 1: Fill the DP table
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= m; ++j) {
                if (s[i - 1] == t[j - 1]) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else {
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }

        // Step 2: Backtrack to find all LCS
        set<string> lcsSet = backtrack(n, m, s, t);

        // Step 3: Convert set to vector
        return vector<string>(lcsSet.begin(), lcsSet.end());
    }
};
```

---

## **Explanation**

### **1. Understanding the Problem**
We are tasked with:
1. Finding the LCS length (which is standard).
2. Extracting all distinct LCS subsequences in **lexicographical order**.

To achieve this:
- We first compute the DP table to determine LCS lengths for each prefix of `s` and `t`.
- We then backtrack through the DP table to reconstruct all LCS strings.

---

### **2. Solution Steps**

#### **Step 1: DP Table Construction**
1. Initialize a 2D DP table `dp[n+1][m+1]`:
   - `dp[i][j]` represents the LCS length for the first `i` characters of `s` and the first `j` characters of `t`.
2. Transition:
   - If `s[i-1] == t[j-1]`: `dp[i][j] = 1 + dp[i-1][j-1]`.
   - Else: `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`.

#### **Step 2: Backtracking to Find All LCS**
1. Start from `(n, m)` and recursively construct all LCS strings:
   - If `s[i-1] == t[j-1]`, add `s[i-1]` to all LCS from `(i-1, j-1)`.
   - Otherwise, explore `(i-1, j)` and `(i, j-1)` **only if they contribute to the LCS length (`dp[i][j]`)**.
2. Use a `set` to store results, ensuring **lexicographical order** automatically.

#### **Step 3: Return Results**
1. Convert the `set<string>` into a `vector<string>` for the final output.

---

## **Time Complexity**

1. **DP Table Construction:**  
   - Filling the DP table requires \(O(n \times m)\).

2. **Backtracking:**
   - Each `(i, j)` state is processed once, and we construct \(k\) strings, where \(k\) is the LCS length.
   - Generating LCS strings involves exploring all valid combinations, with pruning for invalid paths due to memoization.
   - Worst-case time complexity for backtracking is \(O(n \times m \times k)\), where \(k\) is the average length of generated LCS strings.

**Overall Complexity:**  
\(O(n \times m \times k)\).

---

## **Space Complexity**

1. **DP Table:**  
   - \(O(n \times m)\).

2. **Memoization:**  
   - \(O(n \times m)\), as each `(i, j)` state is cached once.

3. **Result Storage:**  
   - \(O(k \times r)\), where \(k\) is the average length of LCS strings, and \(r\) is the total number of LCS.

**Overall Complexity:**  
\(O(n \times m + k \times r)\).

---

## **Dry Run**

### Input:
```plaintext
s = "abaaa"
t = "baabaca"
```

### DP Table:
```
    b a a b a c a
  0 0 0 0 0 0 0 0
a 0 0 1 1 1 1 1 1
b 0 1 1 1 2 2 2 2
a 0 1 2 2 2 3 3 3
a 0 1 2 3 3 3 4 4
a 0 1 2 3 3 3 4 4
```

**LCS Length:**  
`dp[5][7] = 4`.

### Backtracking:
1. Start from `(5, 7)`:
   - Match: `s[4] == t[6] ('a')`, go to `(4, 6)`.
2. Continue backtracking, constructing strings `{"aaaa", "abaa", "baaa"}`.

**Output:**
```plaintext
["aaaa", "abaa", "baaa"]
```

---

## **Summary**
This approach efficiently:
1. Computes the LCS length using a DP table.
2. Extracts all distinct LCS subsequences through memoized backtracking.
3. Guarantees lexicographical order with a `set`.

This separation of concerns makes the solution modular and intuitive.


### **Full Dry Run**

#### Input:
```plaintext
s = "abaaa"
t = "baabaca"
```

---

### **Step 1: Fill the DP Table**

- Initialize a DP table of size \((5+1) \times (7+1)\), where rows correspond to `s` and columns to `t`. All entries are initially 0.
  
#### Transitions:
- Compare each character \(s[i-1]\) with \(t[j-1]\):
  - If \(s[i-1] == t[j-1]\): \(dp[i][j] = 1 + dp[i-1][j-1]\).
  - Else: \(dp[i][j] = \max(dp[i-1][j], dp[i][j-1])\).

---

#### DP Table Construction:

|      | b | a | a | b | a | c | a |
|------|---|---|---|---|---|---|---|
|   0  | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| a    | 0 | 0 | 1 | 1 | 1 | 1 | 1 |
| b    | 0 | 1 | 1 | 1 | 2 | 2 | 2 |
| a    | 0 | 1 | 2 | 2 | 2 | 3 | 3 |
| a    | 0 | 1 | 2 | 3 | 3 | 3 | 4 |
| a    | 0 | 1 | 2 | 3 | 3 | 3 | 4 |

**Final LCS Length: \(dp[5][7] = 4\)**.

---

### **Step 2: Backtrack to Find All LCS**

We will backtrack starting from cell \((5, 7)\), which holds the LCS length \(4\). During backtracking, we explore valid paths to construct all possible LCS strings.

---

#### Backtracking Rules:

1. If \(s[i-1] == t[j-1]\):
   - Add \(s[i-1]\) to all LCS strings from \((i-1, j-1)\).
2. Else:
   - If \(dp[i-1][j] == dp[i][j]\): Explore \((i-1, j)\).
   - If \(dp[i][j-1] == dp[i][j]\): Explore \((i, j-1)\).
3. Use a set to store results to ensure lexicographical order.

---

#### Backtracking Trace:

1. Start at **(5, 7)**:
   - \(s[4] == t[6] ('a')\): Move to \((4, 6)\).
   - Current result: \{"a"\}.

2. At **(4, 6)**:
   - \(s[3] == t[5] ('a')\): Move to \((3, 5)\).
   - Current result: \{"aa"\}.

3. At **(3, 5)**:
   - \(s[2] == t[4] ('a')\): Move to \((2, 3)\).
   - Current result: \{"aaa"\}.

4. At **(2, 3)**:
   - \(s[1] == t[2] ('b')\): Move to \((1, 1)\).
   - Current result: \{"baaa"\}.

5. At **(1, 1)**:
   - \(dp[i-1][j] == dp[i][j]\): Explore \((0, 1)\).
   - \(dp[i][j-1] == dp[i][j]\): Explore \((1, 0)\).

- Backtrack paths generate strings: \{"aaaa", "abaa", "baaa"\}.

---

### **Step 3: Convert Set to Vector**

The backtracking result is stored in a set:
```plaintext
set<string> = {"aaaa", "abaa", "baaa"}
```

Convert this set to a vector:
```plaintext
vector<string> = {"aaaa", "abaa", "baaa"}
```

---

### **Output**

Final output in lexicographical order:
```plaintext
["aaaa", "abaa", "baaa"]
```

---

### **Visualization**

#### DP Table with Backtracking Paths:
- Green: Matched characters contributing to LCS.
- Arrows: Backtracking paths.

```
    b   a   a   b   a   c   a
  0 0   0   0   0   0   0   0
a 0 0  →1→→1→→1→→1→→1→→1→
b 0 ↓1→→1→→1↓→2→→2→→2→
a 0 ↓1→→2→→2→→2↓→3→→3→
a 0 ↓1→→2→→3→→3→→3↓→4→
a 0 ↓1→→2→→3→→3→→3→→4
```

Backtracking paths traverse diagonally for matches (`aaaa`) and explore alternate paths (`abaa`, `baaa`) in lexicographical order.

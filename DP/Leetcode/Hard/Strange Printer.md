### **C++ Code**

```cpp
class Solution {
public:
    int strangePrinter(string s) {
        int n = s.length();
        vector<vector<int>> dp(n, vector<int>(n, -1));
        return solveMem(s, 0, n - 1, dp);
    }
    
    int solveMem(string &s, int start, int end, vector<vector<int>> &dp) {
        if (start > end) return 0; // Agar substring empty ho, toh 0 turns chahiye
        if (dp[start][end] != -1) return dp[start][end]; // Agar already calculate kiya hai toh return karo
        
        // Option 1: Print s[start] alag se aur baki ka substring solve karo
        int res = 1 + solveMem(s, start + 1, end, dp);
        
        // Option 2: Reuse karo agar koi matching character mil jaye
        for (int k = start + 1; k <= end; ++k) {
            if (s[k] == s[start]) {
                res = min(res, solveMem(s, start, k - 1, dp) + solveMem(s, k + 1, end, dp));
            }
        }
        
        return dp[start][end] = res; // Minimum turns return karo
    }
};
```

---

### **Explanation**

#### Problem Samajhte Hain:
1. Ek "strange printer" hai jo sirf ek character ka sequence ek baar me print kar sakta hai.
2. Jab bhi printer print karega, woh kisi bhi range ko cover kar sakta hai (existing characters overwrite hote hain).
3. Tumhe minimum turns nikalne hain jo string `s` ko poora print karne ke liye chahiye.

#### Approach Samjho:
1. **Recursive Breakdown:**
   - `solveMem(s, start, end)` ka matlab hai: `s[start...end]` ko print karne ke liye minimum turns chahiye.
   - Base case: Agar substring empty ho (`start > end`), toh 0 turns chahiye.
   - Do options hain har step par:
     - **Option 1**: `s[start]` ko ek alag turn mein print karo aur baaki substring `s[start+1...end]` ko solve karo.
     - **Option 2**: Agar `s[start]` ka koi matching character `s[k]` mile, toh ek hi turn mein `s[start]` aur `s[k]` ko print kar lo, aur substring ko split kar do.

2. **Dynamic Programming Optimization:**
   - DP table (`dp[start][end]`) ka use karte hain taaki repetitive calculations avoid ho.
   - `dp[start][end]` store karta hai `s[start...end]` ko print karne ke minimum turns.

---

### **Time Complexity**

- **Recursion Tree Analysis**:  
  Har substring ke liye hum `O(end - start)` iterations karte hain (for `k` loop).  
  DP ke wajah se, har substring (`start, end`) ko sirf ek baar solve karte hain.

- **Total Complexity**:  
  Substrings ka total combinations = `O(n^2)`.  
  Har substring solve karne ka time = `O(n)`.  
  **Overall Time Complexity** = `O(n^3)`.

---

### **Space Complexity**

- **DP Table**:  
  2D DP table of size `n x n` store karte hain.  
  Space = `O(n^2)`.

- **Recursion Stack**:  
  Maximum recursion depth = `O(n)`.  
  Additional stack space = `O(n)`.

- **Overall Space Complexity** = `O(n^2)`.

---

### **Dry Run**

#### Input: `s = "aba"`

---

1. **Step 1**: Call `solveMem(s, 0, 2)` (substring `"aba"`)  
   - **Option 1**:  
     Print `s[0] = 'a'` separately and solve `solveMem(s, 1, 2)`.  
     Result = `1 + solveMem(s, 1, 2)`.

   - **Option 2**:  
     Look for `k` where `s[k] == s[0] = 'a'` in the range `[1, 2]`.  
     Find `k = 2`.  
     Result = `solveMem(s, 0, 1) + solveMem(s, 2, 2)`.

   - Minimum = `min(Option 1, Option 2)`.

---

2. **Step 2**: Call `solveMem(s, 1, 2)` (substring `"ba"`)  
   - **Option 1**:  
     Print `s[1] = 'b'` separately and solve `solveMem(s, 2, 2)`.  
     Result = `1 + solveMem(s, 2, 2) = 2`.

   - **Option 2**:  
     No match for `s[1] = 'b'` in range `[2, 2]`.  
     Final result = `2`.

---

3. **Step 3**: Call `solveMem(s, 0, 1)` (substring `"ab"`)  
   - **Option 1**:  
     Print `s[0] = 'a'` separately and solve `solveMem(s, 1, 1)`.  
     Result = `1 + solveMem(s, 1, 1) = 2`.

   - **Option 2**:  
     No match for `s[0] = 'a'` in range `[1, 1]`.  
     Final result = `2`.

---

4. **Step 4**: Call `solveMem(s, 2, 2)` (substring `"a"`)  
   - Single character.  
     Result = `1`.

---

5. **Back to Step 1**: `solveMem(s, 0, 2)`  
   - **Option 1**: `1 + solveMem(s, 1, 2) = 3`.  
   - **Option 2**: `solveMem(s, 0, 1) + solveMem(s, 2, 2) = 2 + 1 = 3`.  
   - Minimum = `min(3, 3) = 2`.

---

### **Final Answer**:  
For `s = "aba"`, minimum turns required = **2**.

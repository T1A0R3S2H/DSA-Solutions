### TL;DR

This code solves the problem of counting valid attendance records of length \( n \) using a recursive + memoization approach. The main idea is to recursively explore all possibilities for adding 'P' (Present), 'L' (Late), and 'A' (Absent) while ensuring constraints are met (fewer than 2 'A' and no 3 consecutive 'L'). Memoization is used to avoid redundant calculations by storing results for each unique state: the remaining days \( n \), whether an 'A' has already been used, and the count of consecutive 'L'.

The time complexity is \( O(n) \) because each state (up to \( n \times 2 \times 3 \)) is computed once. Space complexity is \( O(n \times 2 \times 3) \) due to the 3D DP table. The result is computed modulo \( 10^9+7 \) to handle large values efficiently.

---

### **Explanation (CETSD)**

#### **Code**

```cpp
class Solution {
public:
    const int MOD=1e9+7;
    int solveMem(int n, int absent, int late, vector<vector<vector<int>>>& dp) {
        if (n==0) return 1; // No more states to process
        if (dp[n][absent][late]!=-1) return dp[n][absent][late];
        int count=0; // Count for current state
        count=(count+solveMem(n-1, absent, 0, dp))%MOD; // Add 'P' (Present)
        if (late<2) count=(count+solveMem(n-1, absent, late+1, dp))%MOD; // Add 'L' (Late)
        if (absent==0) count=(count+solveMem(n-1, 1, 0, dp))%MOD; // Add 'A' (Absent)
        return dp[n][absent][late]=count;
    }

    int checkRecord(int n) {
        // 3D DP: n x 2 (absent state) x 3 (late state)
        vector<vector<vector<int>>> dp(n+1, vector<vector<int>>(2, vector<int>(3, -1)));
        return solveMem(n, 0, 0, dp)%MOD;
    }
};
```

---

#### **Explanation**
1. **Recursive Approach**:
   - The function `solveMem` explores all possible ways to build attendance records of length \( n \), ensuring they satisfy the constraints:
     - Fewer than 2 absences ('A').
     - No 3 consecutive lates ('L').
   - At each step, three choices are available:
     1. Add 'P': Resets the count of consecutive 'L'.
     2. Add 'L': Increments the count of consecutive 'L', allowed only if it's less than 2.
     3. Add 'A': Allowed only if no 'A' has been used yet (`absent == 0`).

2. **Base Case**:
   - If \( n == 0 \): All days have been processed, and this is a valid record. Return 1.

3. **Memoization**:
   - A 3D DP table `dp` stores results for each state defined by \( (n, absent, late) \), where:
     - `n`: Remaining days.
     - `absent`: Whether 'A' has been used (0 or 1).
     - `late`: Count of consecutive 'L' days (0 to 2).
   - This avoids redundant computations by caching previously computed results.

4. **Final Calculation**:
   - The final result is the number of valid attendance records of length \( n \), computed as `solveMem(n, 0, 0, dp)`.

---

#### **Time Complexity**
- \( O(n \times 2 \times 3) = O(n) \): Each state \( (n, absent, late) \) is computed once.
- Recursive calls are bounded by the dimensions of the DP table.

#### **Space Complexity**
- \( O(n \times 2 \times 3) \) for the DP table.
- \( O(n) \) additional space for the recursion stack.

---

#### **Dry Run**
**Input**: \( n = 2 \)

1. Initial call: `solveMem(2, 0, 0, dp)`
   - Add 'P': `solveMem(1, 0, 0, dp)`
     - Add 'P': `solveMem(0, 0, 0, dp)` → \( 1 \)
     - Add 'L': `solveMem(0, 0, 1, dp)` → \( 1 \)
     - Add 'A': `solveMem(0, 1, 0, dp)` → \( 1 \)
   - Add 'L': `solveMem(1, 0, 1, dp)`
     - Add 'P': `solveMem(0, 0, 0, dp)` → \( 1 \)
     - Add 'L': `solveMem(0, 0, 2, dp)` → \( 1 \)
   - Add 'A': `solveMem(1, 1, 0, dp)`
     - Add 'P': `solveMem(0, 1, 0, dp)` → \( 1 \)
     - Add 'L': `solveMem(0, 1, 1, dp)` → \( 1 \)

**Output**: \( 8 \) valid records.

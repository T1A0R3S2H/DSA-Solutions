### C++ Code: 
```cpp
class Solution {
public:
    int solveMem(vector<vector<int>>& triangle, int i, int j, vector<vector<int>>& dp){
        int n=triangle.size();
        if(i==n-1) return triangle[n-1][j];
        if(dp[i][j]!=-1) return dp[i][j];
        int down=solveMem(triangle, i+1, j, dp);
        int diag=INT_MAX;
        if (j+1<triangle[i+1].size()) {
            diag=solveMem(triangle, i+1, j+1, dp);
        }
        return dp[i][j]=min(down, diag)+triangle[i][j];
    }
    int minimumTotal(vector<vector<int>>& triangle) {
        int n=triangle.size();
        vector<vector<int>>dp(n+1, vector<int>(n+1, -1));
        return solveMem(triangle, 0, 0, dp);
    }
};
```

---

### **Explanation**
The problem is to find the minimum path sum from the top to the bottom of a triangle array, where at each step, we can move to the adjacent numbers in the row directly below.

1. **Recursive Function (solveMem):**
   - `solveMem` computes the minimum path sum starting at position `(i, j)` in the triangle using recursion and memoization.
   - **Base Case:** 
     - If `i` is the last row (`i == n-1`), return the value at `triangle[n-1][j]` as there are no rows below.
   - **Recursive Case:**
     - Calculate the sum if moving downward (`down`) to `(i+1, j)` and diagonally (`diag`) to `(i+1, j+1)`.
     - Use an `if` condition to check bounds for diagonal access.
     - Store and return the minimum of these two values plus the current triangle value (`triangle[i][j]`).

2. **Top-Down Memoization:**
   - A 2D DP table (`dp`) is used to store intermediate results, avoiding redundant calculations.

3. **Minimum Total Function:**
   - Initializes the DP table with `-1` to signify uncomputed states.
   - Starts the computation from the top `(0, 0)` using the recursive `solveMem` function.

---

### **Time Complexity**
- **Recursive Calls:** Each unique subproblem `(i, j)` is solved once due to memoization.
- **Number of Subproblems:** The total number of subproblems is equal to the number of elements in the triangle, which is `n(n+1)/2` for a triangle with `n` rows.
- **Computational Complexity:** Each subproblem takes \(O(1)\). 
- **Overall Time Complexity:** \(O(n^2)\).

---

### **Space Complexity**
- **DP Table:** Stores \(O(n^2)\) entries.
- **Recursive Stack:** At most `n` calls are present in the stack for a triangle with `n` rows.
- **Overall Space Complexity:** \(O(n^2)\).

---

### **Dry Run**

#### Input:
```cpp
triangle = {{2}, {3, 4}, {6, 5, 7}, {4, 1, 8, 3}};
```

#### Execution:

1. Start at `(0, 0)`: 
   - `solveMem(0, 0)` calls `solveMem(1, 0)` and `solveMem(1, 1)`.
2. For `(1, 0)`:
   - Calls `solveMem(2, 0)` and `solveMem(2, 1)`.
3. For `(2, 0)`:
   - Calls `solveMem(3, 0)` and `solveMem(3, 1)`.
   - Base case returns values: `triangle[3][0] = 4` and `triangle[3][1] = 1`.
   - `dp[2][0] = min(4, 1) + 6 = 7`.
4. For `(2, 1)`:
   - Calls `solveMem(3, 1)` and `solveMem(3, 2)`.
   - Base case returns values: `triangle[3][1] = 1` and `triangle[3][2] = 8`.
   - `dp[2][1] = min(1, 8) + 5 = 6`.
5. Continue backtracking, updating `dp` values:
   - `dp[1][0] = min(7, 6) + 3 = 9`.
   - `dp[1][1] = min(6, 10) + 4 = 10`.
   - `dp[0][0] = min(9, 10) + 2 = 11`.

---

### Output:
The minimum path sum is **11**.

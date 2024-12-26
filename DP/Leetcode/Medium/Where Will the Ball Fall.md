### Code:

```cpp
class Solution {
public:
    int solveMem(vector<vector<int>>& grid, vector<vector<int>>& dp, int row, int col) {
        // If ball reaches bottom, return the column
        if(row == grid.size()) return col;
        
        // If already calculated, return from dp
        if(dp[row][col] != -2) return dp[row][col];
        
        // Calculate next column based on current cell's value
        int nextCol = col + grid[row][col];
        
        // Check if ball gets stuck
        if(nextCol < 0 || nextCol >= grid[0].size() || 
           grid[row][col] != grid[row][nextCol]) {
            return dp[row][col] = -1;
        }
        
        // Continue to next row
        return dp[row][col] = solveMem(grid, dp, row + 1, nextCol);
    }
    
    vector<int> findBall(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        
        // Initialize dp with -2 (to differentiate from -1 which means ball is stuck)
        vector<vector<int>> dp(m, vector<int>(n, -2));
        vector<int> result(n);
        
        // Try dropping ball from each column
        for(int j = 0; j < n; j++) {
            result[j] = solveMem(grid, dp, 0, j);
        }
        
        return result;
    }
};
```

---

### Explanation:

1. **Problem Understanding**:
   - Ek 2D grid hai jisme diagonal boards hain jo balls ko left ya right divert karte hain.
   - Har column se ek ball drop karni hai, aur yeh check karna hai ki ball kahaan land karegi ya kahaan stuck ho jayegi.

2. **Approach**:
   - Ek recursive function `solveMem` banaya hai jo ball ke movement ko trace karta hai.
   - Agar ball grid ke bottom tak pahuch jaati hai, toh uska final column return karte hain.
   - Agar ball wall se takra jaaye ya "V" shape ke pattern mein phase jaaye, toh `-1` return karte hain.
   - Memoization (`dp`) ka use kiya gaya hai taaki overlapping subproblems optimize ho jayein.

3. **Recursive Logic**:
   - Har row par ball ka next column calculate karte hain:
     - `nextCol = col + grid[row][col]`
   - Ball stuck hone ke scenarios:
     - `nextCol` grid ke bahar chala jaaye.
     - Current cell aur next cell ki diagonal values alag ho (matlab "V" shape pattern hai).
   - Agar ball stuck ho jaaye, toh `-1` store karke return karte hain.
   - Nahi toh recursion se next row ke liye call karte hain.

4. **`findBall` Function**:
   - Har column ke liye ball ko drop karte hain aur `solveMem` call karte hain.
   - Result vector mein har column ka output store karte hain.

---

### Time Complexity:
- **Recursive Function**:
  - Har cell ek baar visit hoti hai, toh `O(m * n)` ka time lagta hai (m = rows, n = columns).
- **Overall**:
  - Har column ke liye function call hota hai, toh total complexity bhi `O(m * n)`.

---

### Space Complexity:
- **Memoization Table**:
  - `dp` ka size `m * n` hai, toh `O(m * n)` space lagta hai.
- **Recursive Stack**:
  - Maximum depth `m` hogi, toh `O(m)` stack space lagega.

---

### Dry Run:
#### Input:  
`grid = [[1,1,1,-1,-1], [1,1,1,-1,-1], [-1,-1,-1,1,1], [1,1,1,1,-1], [-1,-1,-1,-1,-1]]`  

1. **Column 0**:
   - Row 0: Ball moves from 0 to 1 (right).
   - Row 1: Ball moves from 1 to 2 (right).
   - Row 2: Stuck at "V" shape (2 to 3 mismatch).
   - Result: `-1`.

2. **Column 1**:
   - Row 0: Ball moves from 1 to 2 (right).
   - Row 1: Stuck at "V" shape (2 to 3 mismatch).
   - Result: `-1`.

3. **Column 4**:
   - Row 0: Ball moves from 4 to 3 (left).
   - Row 1: Stuck at wall (3 out of bounds).
   - Result: `-1`.

**Final Output**: `[1, -1, -1, -1, -1]`

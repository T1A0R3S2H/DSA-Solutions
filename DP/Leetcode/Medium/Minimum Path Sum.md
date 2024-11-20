### Code:
```cpp
class Solution {
public:
    int solveMem(vector<vector<int>>& grid, int i, int j, vector<vector<int>>& dp){
        if (i>=grid.size() || j>=grid[0].size()) return INT_MAX;
        if (i==grid.size()-1 && j==grid[0].size()-1) return grid[i][j];
        if(dp[i][j]!=-1) return dp[i][j];
        int wayDown=solveMem(grid, i+1, j, dp);
        int wayRight=solveMem(grid, i, j+1, dp);
        return dp[i][j]=grid[i][j]+min(wayDown, wayRight);
    }
    int minPathSum(vector<vector<int>>& grid) {
        int m=grid.size();
        int n=grid[0].size();
        vector<vector<int>>dp(m+1, vector<int>(n+1, -1));
        return solveMem(grid, 0, 0, dp);
    }
};
```
### Explanation:
The problem involves finding the minimum path sum in a grid from the top-left to the bottom-right corner, moving only down or right. The solution uses a recursive + memoization approach (`solveMem`) to calculate the minimum path sum from any cell `(i, j)` to the destination.

#### Steps:
1. **Base Cases**:
   - If `i` or `j` exceeds the grid bounds, return `INT_MAX` (invalid path).
   - If the current cell is the bottom-right corner `(m-1, n-1)`, return its value (`grid[i][j]`).

2. **Memoization**:
   - If the result for a cell `(i, j)` is already computed (`dp[i][j] != -1`), return it to avoid redundant calculations.

3. **Recursive Calls**:
   - The function explores two paths:
     - Downward: `solveMem(grid, i+1, j, dp)`
     - Rightward: `solveMem(grid, i, j+1, dp)`
   - The minimum of these two is added to the current cell value (`grid[i][j]`) to determine the result for `(i, j)`.

4. **Memoization Table Initialization**:
   - A `dp` table of size `(m+1) x (n+1)` is initialized with `-1` to store the minimum path sums for subproblems.

5. **Start Calculation**:
   - The function starts at `(0, 0)` and recursively computes the minimum path sum.

---

### Time Complexity:
- **O(m × n)**: Each cell `(i, j)` is computed only once, thanks to memoization.

### Space Complexity:
- **O(m × n)**: For the `dp` table.
- **O(m + n)**: For the recursion stack, representing the depth of the recursive calls.

---

### Dry Run:
#### Input: `grid = [[1, 3, 1], [1, 5, 1], [4, 2, 1]]`
1. **Initialization**:
   - `m = 3`, `n = 3`
   - `dp` table is `3 x 3`, initialized with `-1`.

2. **Recursive Execution**:
   - Start at `(0, 0)`:
     - Calculate `wayDown = solveMem(1, 0, dp)` and `wayRight = solveMem(0, 1, dp)`.
     - At `(1, 0)`:  
       - Compute `wayDown = solveMem(2, 0, dp)` and `wayRight = solveMem(1, 1, dp)`.
       - At `(2, 0)`:
         - Compute `wayDown = solveMem(3, 0, dp)` (out of bounds, returns `INT_MAX`).
         - Compute `wayRight = solveMem(2, 1, dp)`.
         - At `(2, 1)`:
           - Compute `wayDown = solveMem(3, 1, dp)` (out of bounds).
           - Compute `wayRight = solveMem(2, 2, dp)` (base case, return `1`).
           - `dp[2][1] = 2 + min(INT_MAX, 1) = 3`.
         - `dp[2][0] = 4 + min(INT_MAX, 3) = 7`.
       - At `(1, 1)`:
         - Compute `wayDown = solveMem(2, 1, dp)` (memoized, `3`).
         - Compute `wayRight = solveMem(1, 2, dp)`.
         - At `(1, 2)`:
           - Compute `wayDown = solveMem(2, 2, dp)` (base case, `1`).
           - Compute `wayRight = solveMem(1, 3, dp)` (out of bounds).
           - `dp[1][2] = 1 + min(1, INT_MAX) = 2`.
         - `dp[1][1] = 5 + min(3, 2) = 7`.
       - `dp[1][0] = 1 + min(7, 7) = 8`.
     - At `(0, 1)`:
       - Compute similar results and eventually find `dp[0][0] = 7`.

3. **Result**:
   - `minPathSum = dp[0][0] = 7`.

---

### Final Output:
For the input `grid = [[1, 3, 1], [1, 5, 1], [4, 2, 1]]`, the minimum path sum is **7**.

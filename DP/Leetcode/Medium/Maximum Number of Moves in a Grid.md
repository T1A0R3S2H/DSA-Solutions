# 🔥Recursion | Recursion+Memoization | Tabulation
### Code:
```cpp
class Solution {
public:
    int solveRec(vector<vector<int>>& grid, int row, int col) {
        int m=grid.size();
        int n=grid[0].size();

        // out of bounds
        if (row<0 || row>=m || col>=n) return 0;

        int curr=grid[row][col];
        int way1=0, way2=0, way3=0;
        if (row>0 && col+1<n && grid[row-1][col+1]>curr) {
            way1=1+solveRec(grid, row-1, col+1);
        }
        if (col+1<n && grid[row][col+1]>curr) {
            way2=1+solveRec(grid, row, col+1);
        }
        if (row+1<m && col+1<n && grid[row+1][col+1]>curr) {
            way3=1+solveRec(grid, row+1, col+1);
        }
        return max(way1, max(way2, way3));
    }

    int solveMem(vector<vector<int>>& grid, int row, int col, vector<vector<int>>& dp){
        int m=grid.size();
        int n=grid[0].size();

        // out of bounds
        if (row<0 || row>=m || col < 0 || col>=n) return 0;

        if(dp[row][col]!=-1) return dp[row][col];

        int curr=grid[row][col];
        int way1=0, way2=0, way3=0;
        if (row>0 && col+1<n && grid[row-1][col+1]>curr) {
            way1=1+solveMem(grid, row-1, col+1, dp);
        }
        if (col+1<n && grid[row][col+1]>curr) {
            way2=1+solveMem(grid, row, col+1, dp);
        }
        if (row+1<m && col+1<n && grid[row+1][col+1]>curr) {
            way3=1+solveMem(grid, row+1, col+1, dp);
        }
        return dp[row][col]=max(way1, max(way2, way3));
    }

    int solveTab(vector<vector<int>>& grid){
        int m=grid.size();
        int n=grid[0].size();
        vector<vector<int>> dp(m, vector<int>(n, 0));

        for (int col=n-2; col>=0; col--) {
            for (int row=0; row<m; row++) {
                
                if (row>0 && grid[row-1][col+1]>grid[row][col]) {
                    dp[row][col]=max(dp[row][col], 1+dp[row-1][col+1]);
                }
                if (grid[row][col+1]>grid[row][col]) {
                    dp[row][col]=max(dp[row][col], 1+dp[row][col+1]);
                }
                if (row<m-1 && grid[row+1][col+1]>grid[row][col]) {
                    dp[row][col]=max(dp[row][col], 1+dp[row+1][col+1]);
                }
            }
        }
        int maxi=0;
        for (int i=0; i<m; i++) {
            maxi=max(maxi, dp[i][0]);
        }
        return maxi;
    }

    int maxMoves(vector<vector<int>>& grid) {
        return solveTab(grid);
    }
};
```

### Explanation
The code implements a solution to the problem of finding the maximum number of moves in a grid where you can only move to cells with a strictly greater value. It uses three approaches: recursion, memoization, and tabulation. The primary function, `maxMoves`, calls the tabulation method `solveTab` to compute the maximum number of moves starting from any cell in the first column.

### Time Complexity
- **Recursion:** O(3^(m * n)), where m is the number of rows and n is the number of columns. This is due to the three recursive calls for each cell.
- **Memoization:** O(m * n), since it stores the results of previously computed states.
- **Tabulation:** O(m * n), as it fills up a DP table once for the entire grid.

### Space Complexity
- **Recursion:** O(m + n), for the recursion stack in the worst case.
- **Memoization:** O(m * n), for the DP table used to store results.
- **Tabulation:** O(m * n), for the DP table used to store results.

### Dry Run
Consider the grid:

```
grid = [
    [2, 4, 3, 5],
    [5, 4, 9, 3],
    [3, 4, 2, 11],
    [10, 9, 13, 15]
]
```

1. **Tabulation Process:**
   - Start filling the DP table from the second last column to the first.
   - For each cell, check if you can move up-right, right, or down-right based on the conditions.
   - Update the DP table accordingly.

2. **Final Result:**
   - After processing, the maximum number of moves starting from the first column is computed and returned.

---

# 🔥Space Optimised Approach
### Code:

```cpp
class Solution {
public:
    int maxMoves(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        vector<int> prev(n, 0); // Previous row
        vector<int> curr(n, 0); // Current row

        for (int col = n - 2; col >= 0; col--) {
            for (int row = 0; row < m; row++) {
                curr[row] = 0; // Reset current row for this column
                
                // Check if moving to the upper-right cell is valid
                if (row > 0 && grid[row - 1][col + 1] > grid[row][col]) {
                    curr[row] = max(curr[row], 1 + prev[row - 1]);
                }
                // Check if moving to the right cell is valid
                if (grid[row][col + 1] > grid[row][col]) {
                    curr[row] = max(curr[row], 1 + prev[row]);
                }
                // Check if moving to the lower-right cell is valid
                if (row < m - 1 && grid[row + 1][col + 1] > grid[row][col]) {
                    curr[row] = max(curr[row], 1 + prev[row + 1]);
                }
            }
            prev = curr; // Update previous row for the next iteration
        }
        
        int maxi = 0;
        for (int i = 0; i < m; i++) {
            maxi = max(maxi, prev[i]); // Get the maximum moves from the last processed column
        }
        return maxi;
    }
};
```

### Explanation
- **Purpose:** This function computes the maximum number of moves from any cell in the first column of the grid while optimizing space by only storing two rows of the DP table.
- **Initialization:** Two vectors, `prev` and `curr`, are initialized to store the number of moves for the previous and current rows, respectively.
- **Main Loop:**
  - Iterate backward over the columns.
  - For each cell in the current column, check if moving to the upper-right, right, or lower-right cells is valid and update `curr[row]` accordingly.
- **Update Rows:** After processing each column, update the `prev` vector to the current vector for the next iteration.
- **Final Result:** The maximum moves are determined from the `prev` vector after all columns are processed.

### Time Complexity
- **O(m * n):** We still iterate through each cell in the grid once.

### Space Complexity
- **O(n):** Instead of maintaining a 2D DP array, we now only use two 1D arrays, significantly reducing space usage.

### Dry Run Example
Given the same grid:

```
grid = [
    [2, 4, 3, 5],
    [5, 4, 9, 3],
    [3, 4, 2, 11],
    [10, 9, 13, 15]
]
```

1. Start processing from the second last column (index 2) to the first (index 0).
2. For each cell in the current column, determine the number of moves possible based on values from the previous column (using `prev`).
3. Update the maximum moves for the entire grid at the end of the column processing.

If you have more questions or need further assistance, feel free to ask!

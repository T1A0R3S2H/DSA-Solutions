### Code: 

```cpp
class Solution {
public:
    int solveMem(int i, int j, vector<vector<int>>& obstacleGrid, vector<vector<int>>& dp) {
        int m=obstacleGrid.size();
        int n=obstacleGrid[0].size();
        if (i>=m || j>=n || obstacleGrid[i][j]==1){
            return 0; //jab bhi obstacle, then move to another route
        } 
        if (i==m-1 && j==n-1) return 1;
        if (dp[i][j]!=-1) return dp[i][j];
        dp[i][j]=solveMem(i+1, j, obstacleGrid, dp)+solveMem(i, j+1, obstacleGrid, dp);
        return dp[i][j];
    }

    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m=obstacleGrid.size();
        int n=obstacleGrid[0].size();
        vector<vector<int>> dp(m, vector<int>(n, -1));
        return solveMem(0, 0, obstacleGrid, dp);
    }
};
```

---

### Explanation

1. **Base Cases**:
   - If the cell is out of bounds or contains an obstacle (`obstacleGrid[i][j] == 1`), return `0`.
   - If the robot reaches the destination `(m-1, n-1)`, return `1`.

2. **Memoization**:
   - Use a `dp[i][j]` table to store the result of subproblems:
     - `dp[i][j]` represents the number of unique paths from `(i, j)` to `(m-1, n-1)`.
   - If `dp[i][j] != -1`, return the precomputed result.

3. **Recursive Relation**:
   - From any cell `(i, j)`, the total paths are the sum of:
     - Paths from the cell below: `solveMem(i + 1, j, obstacleGrid, dp)`
     - Paths from the cell to the right: `solveMem(i, j + 1, obstacleGrid, dp)`
   - Store this result in `dp[i][j]`.

4. **Initialization**:
   - Initialize the `dp` table with `-1` to indicate uncomputed states.

---

### Time Complexity

- **O(m * n)**:
  - Each cell `(i, j)` is visited and computed only once due to memoization.
  - The `dp` table contains `m * n` cells, and filling each cell takes constant time.

---

### Space Complexity

- **O(m * n)**: For the `dp` table used to store results for all states.
- **O(m + n)**: For the recursion stack, as the maximum depth of recursion occurs when traversing diagonally down the grid.

---

### Dry Run

**Input**: `obstacleGrid = [[0, 0, 0], [0, 1, 0], [0, 0, 0]]`

#### Recursive Calls:
1. Start at `(0, 0)`:
   - `solveMem(0, 0)` calls:
     - `solveMem(1, 0)` (down)
     - `solveMem(0, 1)` (right)

2. Process obstacles:
   - When reaching `(1, 1)`, return `0` as it contains an obstacle.

3. Backtrack and compute `dp`:
   - Combine results of right and down paths for each cell.
   - Example: `dp[2][2] = 1`, `dp[1][2] = 1`, `dp[0][2] = 1`, etc.

4. Final result is stored in `dp[0][0]`, which equals `2`.

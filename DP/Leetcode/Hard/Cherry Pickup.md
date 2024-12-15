#### **Code**
```cpp
class Solution {
public:
    int solveMem(vector<vector<int>>& grid, int r1, int c1, int r2, vector<vector<vector<int>>>& dp) {
        int c2 = r1 + c1 - r2; // Derived from the relation c1 + r1 = c2 + r2

        // Base cases: Out of bounds or blocked cell
        if (r1 >= grid.size() || c1 >= grid[0].size() || r2 >= grid.size() || c2 >= grid[0].size() ||
            grid[r1][c1] == -1 || grid[r2][c2] == -1) {
            return INT_MIN;
        }

        // If both paths reach the bottom-right corner
        if (r1 == grid.size() - 1 && c1 == grid[0].size() - 1) {
            return grid[r1][c1];
        }

        // Memoization
        if (dp[r1][c1][r2] != -1) {
            return dp[r1][c1][r2];
        }

        // Collect cherries for the current cells
        int cherries = 0;
        if (r1 == r2 && c1 == c2) {
            cherries = grid[r1][c1];
        } else {
            cherries = grid[r1][c1] + grid[r2][c2];
        }

        // Explore all possible moves for both paths
        int dir1 = solveMem(grid, r1 + 1, c1, r2 + 1, dp); // Both move down
        int dir2 = solveMem(grid, r1, c1 + 1, r2, dp);     // First moves right, second moves down
        int dir3 = solveMem(grid, r1 + 1, c1, r2, dp);     // First moves down, second moves right
        int dir4 = solveMem(grid, r1, c1 + 1, r2 + 1, dp); // Both move right

        // Choose the best among all moves
        cherries += max({dir1, dir2, dir3, dir4});

        return dp[r1][c1][r2] = cherries;
    }

    int cherryPickup(vector<vector<int>>& grid) {
        int n = grid.size();
        vector<vector<vector<int>>> dp(n, vector<vector<int>>(n, vector<int>(n, -1)));
        int result = solveMem(grid, 0, 0, 0, dp);
        return max(0, result);
    }
};
```

---

#### **Explanation**
This is a dynamic programming (DP) solution to the **Cherry Pickup** problem using recursion and memoization. The idea is to simulate two players (or paths) traversing the grid simultaneously and picking up cherries. Each path starts at `(0,0)` and ends at `(n-1,n-1)`.

1. **Paths Representation**:
   - First path: `(r1, c1)`
   - Second path: `(r2, c2)` where `c2 = r1 + c1 - r2` ensures both paths are synchronized (i.e., `r1 + c1 = r2 + c2`).

2. **Base Cases**:
   - Out-of-bounds or blocked cells (`-1`) return `INT_MIN` (invalid paths).
   - If both paths reach the bottom-right cell `(n-1, n-1)`, return the cherry value at that cell.

3. **Recursion**:
   - Calculate cherries collected by both paths. If they overlap, count the cherries only once.
   - Explore all four possible combinations of moves for the two paths:
     - Both move down.
     - First moves right, second moves down.
     - First moves down, second moves right.
     - Both move right.
   - Add the maximum cherries from these moves to the current cherries.

4. **Memoization**:
   - Use a 3D `dp` array to store the results of `(r1, c1, r2)` states to avoid redundant calculations.

5. **Final Result**:
   - If no valid path exists, return `0`. Otherwise, return the maximum cherries collected.

---

#### **Time Complexity**
- The state is defined by `(r1, c1, r2)`, where:
  - `r1` and `r2` can take values from `0` to `n-1`.
  - `c1` and `c2` are determined by `r1, c1, r2` (`c2 = r1 + c1 - r2`).
- Total states: \( O(n^3) \).
- Each state takes \( O(1) \) for computation.
- **Total Time Complexity**: \( O(n^3) \).

---

#### **Space Complexity**
- 3D DP array: \( O(n^3) \).
- Recursive stack space: \( O(n^2) \) (since two pointers traverse up to `n`).
- **Total Space Complexity**: \( O(n^3) \).

---

#### **Dry Run**
Given grid:
```
0 1 -1
1 0 -1
1 1  1
```

---

**Step 1: Starting from `(0,0)` for both paths**
- `r1 = 0, c1 = 0`
- `r2 = 0, c2 = 0`
- Collect `grid[0][0] = 0`. No overlap as both are on `(0,0)`.

---

**Step 2: First recursive call**
- Explore all moves for both paths.

**Move 1**: Both down  
- `r1 = 1, c1 = 0`, `r2 = 1, c2 = 0`
- Collect `grid[1][0] + grid[1][0] = 1`.

---

**Move 2**: First right, second down  
- `r1 = 0, c1 = 1`, `r2 = 1, c2 = 0`
- Invalid as `grid[0][1] = 1`, `grid[1][0] = 1`.

---

**Continue exploring...**

---

Let me know if you want detailed dry-run with all steps explicitly written!

### Code
```cpp
class Solution {
public:
    int solveMem(int i, int j, int n, int m, vector<vector<int>>& dp) {
        if (i==(n-1) && j==(m-1)) return 1;
        if (i>=n || j>=m) return 0;
        if (dp[i][j]!=-1) return dp[i][j];
        dp[i][j]=solveMem(i+1, j, n, m, dp)+solveMem(i, j+1, n, m, dp);
        return dp[i][j];
    }

    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m, vector<int>(n, -1));
        return solveMem(0, 0, m, n, dp);
    }
};
```
### Explanation

**Problem Understanding:**
We need to calculate the number of unique paths from the top-left corner `(0, 0)` to the bottom-right corner `(m-1, n-1)` in an `m x n` grid. Movement is restricted to either **right** or **down** at each step. 

---

**Memoized Approach:**
The recursive solution is optimized using **memoization** to avoid redundant calculations. 

1. **Base Cases**:
   - If the current cell `(i, j)` is the bottom-right corner `(n-1, m-1)`, return `1` since we have successfully reached the destination.
   - If `(i, j)` goes out of bounds (`i >= n` or `j >= m`), return `0` as it's an invalid path.

2. **Memoization**:
   - Use a 2D `dp` table where `dp[i][j]` stores the number of unique paths from `(i, j)` to `(n-1, m-1)`.
   - If `dp[i][j] != -1`, it means the result for this state is already computed, and we return it directly.

3. **Recursive Relation**:
   - At each cell `(i, j)`, the total paths are the sum of:
     - Paths from the cell below: `solveMem(i + 1, j, n, m, dp)`
     - Paths from the cell to the right: `solveMem(i, j + 1, n, m, dp)`
   - Store this result in `dp[i][j]`.

4. **Initialization**:
   - Initialize the `dp` table with `-1` to indicate uncomputed states.

---

### Time Complexity

- **O(m * n)**: 
  - Each cell `(i, j)` is visited and computed at most once due to memoization.
  - The `dp` table contains `m * n` cells, and filling each cell takes constant time.

---

### Space Complexity

- **O(m * n)**: For the `dp` table used to store results for all states.
- **O(m + n)**: For the recursion stack, as the maximum depth of recursion occurs when traversing diagonally down the grid.

---

### Dry Run

**Input**: `m = 3, n = 3`

#### Initialization:
- `dp` is a `3x3` matrix initialized with `-1`:

```
dp = [[-1, -1, -1],
      [-1, -1, -1],
      [-1, -1, -1]]
```

#### Recursive Calls:
1. Start at `(0, 0)`:
   - `solveMem(0, 0, 3, 3, dp)` calls:
     - `solveMem(1, 0, 3, 3, dp)` (down)
     - `solveMem(0, 1, 3, 3, dp)` (right)

2. Compute `solveMem(1, 0, 3, 3, dp)`:
   - Calls:
     - `solveMem(2, 0, 3, 3, dp)` (down)
     - `solveMem(1, 1, 3, 3, dp)` (right)

3. Continue until reaching `(2, 2)`:
   - Base case: Return `1` for destination.

4. Backtrack and update `dp`:
   - Example:
     - `dp[2][1] = dp[2][2] + dp[3][1] = 1 + 0 = 1`
     - `dp[1][1] = dp[2][1] + dp[1][2] = 1 + 1 = 2`
   - Final `dp[0][0] = 6`.

#### Result:
- `uniquePaths(3, 3)` returns `6`.

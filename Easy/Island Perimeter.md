### Code
```cpp
class Solution {
public:
    int islandPerimeter(vector<vector<int>>& grid) {
        int ans = 0;
        int m = grid.size();
        int n = grid[0].size();
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    // Count edges that are not shared with adjacent land cells
                    int count = 4;
                    if (i > 0 && grid[i - 1][j] == 1) {
                        count--;
                    }
                    if (i < m - 1 && grid[i + 1][j] == 1) {
                        count--;
                    }
                    if (j > 0 && grid[i][j - 1] == 1) {
                        count--;
                    }
                    if (j < n - 1 && grid[i][j + 1] == 1) {
                        count--;
                    }
                    ans += count;
                }
            }
        }
        return ans;
    }
};
```
### Key Points:
- You iterate through the entire grid, and for each land cell (`grid[i][j] == 1`), you initially assume it has 4 edges.
- Then, for each neighboring land cell (up, down, left, right), you reduce the count of the perimeter by 1 because the shared edge between two land cells reduces the perimeter.
- Finally, you sum all the perimeters for the entire grid to get the total.

### Code Walkthrough:

1. **Grid dimensions:**
   ```cpp
   int m = grid.size();  // number of rows
   int n = grid[0].size();  // number of columns
   ```
   Here, `m` is the number of rows and `n` is the number of columns in the grid.

2. **Iterating over the grid:**
   ```cpp
   for (int i = 0; i < m; i++) {
       for (int j = 0; j < n; j++) {
           if (grid[i][j] == 1) {
   ```
   You iterate over each cell in the grid. If the cell represents land (`grid[i][j] == 1`), you proceed to check its neighbors.

3. **Checking neighbors and counting edges:**
   - Each cell starts with a perimeter count of 4 (one for each side).
   - For each adjacent land cell (above, below, left, right), you reduce the perimeter by 1.
   - You only check boundaries where `i > 0`, `i < m - 1`, `j > 0`, and `j < n - 1` to avoid out-of-bound errors.

4. **Perimeter update:**
   After checking all the neighbors for each land cell, you update the total perimeter:
   ```cpp
   ans += count;
   ```

### Complexity:
- **Time Complexity:** `O(m * n)` where `m` is the number of rows and `n` is the number of columns. You are visiting each cell once.
- **Space Complexity:** `O(1)` because you're only using a few variables for counting.

### Example:

- For input `[[0,1,0,0],[1,1,1,0],[0,1,0,0],[1,1,0,0]]`, the expected output is `16`. The algorithm correctly counts the number of non-shared edges, giving the total perimeter.

### Final Thoughts:
This solution is optimal for the constraints provided (1 ≤ row, col ≤ 100), and the logic is easy to follow. The constraints ensure that the problem size remains manageable within the time limits.

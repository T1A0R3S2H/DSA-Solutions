### Code
```cpp
class Solution {
public:
    // DFS function to mark land cells as visited
    void dfs(vector<vector<int>>& grid, int i, int j) {
        if (i<0 || i>=grid.size() || j<0 || j>=grid[0].size() || grid[i][j]==0) return;

        // Mark the cell as visited (set it to 0)
        grid[i][j]=0;

        // Recursively apply DFS in four directions
        dfs(grid, i-1, j); // Up
        dfs(grid, i+1, j); // Down
        dfs(grid, i, j-1); // Left
        dfs(grid, i, j+1); // Right
    }

    int numEnclaves(vector<vector<int>>& grid) {
        int m=grid.size();
        int n=grid[0].size();
        
        // Run DFS for boundary land cells (first and last row, first and last column)
        for (int i=0; i< m; i++) {
            for (int j= 0; j<n; j++) {
                // If it's a boundary cell and it's land, apply DFS to mark it as visited
                if (i==0 || i==m-1 || j==0 || j==n-1) {
                    if (grid[i][j]==1) {
                        dfs(grid, i, j);
                    }
                }
            }
        }

        // Now count the remaining land cells which are enclaves
        int count=0;
        for (int i=0; i<m; i++) {
            for (int j=0; j<n; j++) {
                if (grid[i][j]==1) {
                    count++;
                }
            }
        }

        return count;
    }
};

```
### Explanation

The problem asks us to find the number of enclaves in a binary grid, where `0` represents water and `1` represents land. A land cell is considered an "enclave" if it cannot move off the boundary of the grid in any number of moves.

The solution uses **Depth-First Search (DFS)** to mark the land cells that can reach the boundary, making those cells no longer considered enclaves. Here’s the breakdown of the approach:

1. **Mark Boundary Land Cells**: 
   - We perform a DFS starting from all land cells (`1`s) located on the boundary (i.e., cells in the first and last rows or columns). Any land cell reachable from the boundary is marked as visited by turning it into water (`0`).
   
2. **Count Enclosed Land Cells**:
   - After running DFS for all boundary cells, the only land cells left (`1`s) in the grid are those that are completely enclosed by water and can't escape to the boundary. These are the enclaves, and we count them.

### Time Complexity

- **DFS Traversal**: We visit each cell at most once during the DFS, and there are \( m \times n \) cells in total, where \( m \) is the number of rows and \( n \) is the number of columns. Thus, the time complexity for DFS is:
  - **O(m \* n)**

- **Enclave Counting**: After the DFS, we make one more pass through the grid to count the remaining land cells, which also takes \( O(m \times n) \).

Thus, the total time complexity is:
- **O(m \* n)**.

### Space Complexity

- **DFS Recursion Stack**: In the worst case, the DFS may go as deep as the total number of cells in the grid. The space used by the recursion stack is proportional to the size of the grid, which is \( O(m \times n) \) in the worst case.
- **Grid Storage**: The grid itself uses \( O(m \times n) \) space.

Thus, the total space complexity is:
- **O(m \* n)**.

### Dry Run

Let's dry run the algorithm on the following example:

#### Input Grid:

```
[
    [0, 0, 0, 0],
    [1, 0, 1, 0],
    [0, 1, 1, 0],
    [0, 0, 0, 0]
]
```

1. **Initial State**: 
   - The grid contains 1s that represent land cells and 0s that represent water.

2. **Mark Boundary Cells**:
   - We iterate over the boundary cells:
     - Row 1, Column 1 (`[0,0]`): Water, skip.
     - Row 1, Column 2 (`[0,1]`): Water, skip.
     - Row 1, Column 3 (`[0,2]`): Water, skip.
     - Row 1, Column 4 (`[0,3]`): Water, skip.
     - Row 2, Column 1 (`[1,0]`): Land, but part of the boundary, so we apply DFS. This cell is surrounded by water and will be marked as visited (turn to `0`).
     - Continue for all boundary cells.
   
   After processing the boundary cells, the grid becomes:
   ```
   [
       [0, 0, 0, 0],
       [0, 0, 1, 0],
       [0, 1, 1, 0],
       [0, 0, 0, 0]
   ]
   ```

3. **Count Enclaves**:
   - Now, we iterate through the grid again and count the remaining land cells (`1`s) that are not connected to the boundary. These cells are enclosed and cannot escape to the boundary.
   
   There are **3 land cells** left, which are the enclaves.

#### Output: `3`

### Summary

- **Time Complexity**: \( O(m \times n) \)
- **Space Complexity**: \( O(m \times n) \)

This solution efficiently marks boundary-connected land cells and counts the remaining enclaves in the grid.

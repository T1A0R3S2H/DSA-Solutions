### Code:
```cpp
class Solution {
public:
    void dfs(vector<vector<char>>& grid, int i, int j) {
        int m=grid.size();
        int n=grid[0].size();
        if (i<0 || i>=m || j<0 || j>=n || grid[i][j]=='0') {
            return;
        }
        // Mark the current cell as visited
        grid[i][j] = '0';
        
        dfs(grid, i-1, j); // Up
        dfs(grid, i+1, j); // Down
        dfs(grid, i, j-1); // Left
        dfs(grid, i, j+1); // Right
    }
    
    int numIslands(vector<vector<char>>& grid) {
        int count=0;
        int m=grid.size();
        int n=grid[0].size();
        for (int i=0; i<m; i++) {
            for (int j=0; j<n; j++) {
                // If we find an unvisited land cell ('1')
                if (grid[i][j]=='1') {
                    count++;
                    // Perform DFS to mark the entire island as visited
                    dfs(grid, i, j);
                }
            }
        }
        return count;
    }
};
```

### Explanation

The problem is to count the number of islands in a 2D grid where:
- `'1'` represents land.
- `'0'` represents water.
An island is a group of adjacent lands (horizontally or vertically connected).

### Approach:
- The algorithm uses Depth-First Search (DFS) to explore each island.
- It iterates through the entire grid, and each time a `'1'` (unvisited land) is encountered, it triggers a DFS. The DFS marks the entire connected component (island) as visited (changing it from `'1'` to `'0'`).
- After the DFS completes, the island count is incremented, and the search continues for more islands.

### Detailed Steps:
1. **Iterate through the grid**: For each cell, if it is a `'1'`, it signifies the start of a new island.
2. **DFS function**: For each new island, the DFS function is invoked. This function explores all connected cells in four directions (up, down, left, and right), marking them as visited by changing `'1'` to `'0'`.
3. **Island count**: Every time a new island is found, the count is incremented.

### Dry Run

Let's dry-run the algorithm on the second example input grid:

```
Input:
grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
```

- Start iterating through the grid at (0, 0). It is a `'1'`, so we start a DFS to explore the first island.
- The DFS marks all connected `'1'` cells as visited (`'0'`), covering (0,0), (0,1), (1,0), (1,1).
- After the first DFS, the count of islands is `1`.
- Continue iterating. At (2, 2), we find another `'1'`. Start a new DFS to mark this second island. Only (2,2) is marked as visited.
- The count of islands is now `2`.
- Continue iterating. At (3, 3), we find a `'1'`. Start a DFS to explore the third island, marking (3,3) and (3,4) as visited.
- The final count of islands is `3`.

### Time Complexity

- **DFS Complexity**: Each DFS visits every land cell (`'1'`) exactly once. When a DFS is triggered, it marks all adjacent land cells as visited.
- In the worst case, we may visit all cells in the grid once, either when traversing or marking them.
- The outer loop iterates through each cell, and each cell is processed by DFS once.
- **Time complexity** is therefore \( O(m \times n) \), where \( m \) is the number of rows, and \( n \) is the number of columns in the grid.

### Space Complexity

- **Space complexity** comes from the recursion stack in DFS. In the worst case, the DFS stack could grow to a maximum depth equal to the total number of cells in the grid (i.e., when the entire grid is one big island).
- The maximum recursion depth for DFS could be the number of land cells, which is \( O(m \times n) \).
- However, in practice, the stack space will depend on the structure of the islands (e.g., long narrow islands will use more space).

Thus, **space complexity** is \( O(m \times n) \) in the worst case due to recursion.

### Summary:
- **Time Complexity**: \( O(m \times n) \), where \( m \) is the number of rows and \( n \) is the number of columns in the grid.
- **Space Complexity**: \( O(m \times n) \), due to the DFS recursion stack in the worst case.


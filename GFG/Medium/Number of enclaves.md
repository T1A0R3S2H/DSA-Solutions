### Code
```cpp
class Solution {
  public:
  
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
    
    int numberOfEnclaves(vector<vector<int>> &grid) {
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

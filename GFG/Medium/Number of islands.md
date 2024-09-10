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
        dfs(grid, i-1, j-1);
        dfs(grid, i-1, j+1);
        dfs(grid, i+1, j-1);
        dfs(grid, i+1, j+1);
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

```c
class Solution {
  public:
    // Directions array to move up, down, left, and right
    vector<pair<int, int>> directions = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
    
    bool dfs(vector<vector<int>>& heights, vector<vector<bool>>& visited, int maxEffort, int x, int y) {
        int rows = heights.size();
        int columns = heights[0].size();
        
        // If we've reached the bottom-right corner, return true
        if (x == rows - 1 && y == columns - 1) return true;
        
        visited[x][y] = true;
        
        // Traverse all four directions
        for (auto& dir : directions) {
            int newX = x + dir.first;
            int newY = y + dir.second;
            
            // Check if the new coordinates are within bounds
            if (newX >= 0 && newX < rows && newY >= 0 && newY < columns && !visited[newX][newY]) {
                int diff = abs(heights[newX][newY] - heights[x][y]);
                
                // If the current height difference is within the maxEffort limit, continue the DFS
                if (diff <= maxEffort) {
                    if (dfs(heights, visited, maxEffort, newX, newY)) return true;
                }
            }
        }
        
        return false;
    }
    
    int MinimumEffort(int rows, int columns, vector<vector<int>>& heights) {
        // Binary search based approach to find minimum effort
        
        // Define the range of efforts
        int left = 0;
        int right = 1e6;
        
        while (left < right) {
            int mid = (left + right) / 2;
            vector<vector<bool>> visited(rows, vector<bool>(columns, false));
            
            // If DFS finds a path with the current maximum effort, decrease the right boundary
            if (dfs(heights, visited, mid, 0, 0)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        
        return left;
    }
};

```

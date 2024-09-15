```c
#include <vector>
#include <queue>
using namespace std;

class Solution {
  public:
    int shortestPath(vector<vector<int>> &grid, pair<int, int> source,
                     pair<int, int> destination) {
        int n = grid.size();
        int m = grid[0].size();
        int drow[] = {-1, 1, 0, 0}; // Up, Down, Left, Right
        int dcol[] = {0, 0, -1, 1}; // Up, Down, Left, Right

        // Extract source and destination positions
        int startRow = source.first;
        int startCol = source.second;
        int endRow = destination.first;
        int endCol = destination.second;

        // Edge case: if source or destination are invalid or blocked
        if (grid[startRow][startCol] != 1 || grid[endRow][endCol] != 1) return -1;

        // Initialize BFS
        queue<pair<int, pair<int, int>>> q;
        q.push({0, {startRow, startCol}});
        vector<vector<bool>> visited(n, vector<bool>(m, false));
        visited[startRow][startCol] = true;

        while (!q.empty()) {
            auto it = q.front();
            q.pop();
            int dist = it.first;
            int row = it.second.first;
            int col = it.second.second;

            // Check if we reached the destination
            if (row == endRow && col == endCol) return dist;

            // Explore the four directions
            for (int i = 0; i < 4; i++) {
                int nrow = row + drow[i];
                int ncol = col + dcol[i];
                if (nrow >= 0 && nrow < n && ncol >= 0 && ncol < m &&
                    grid[nrow][ncol] == 1 && !visited[nrow][ncol]) {
                    visited[nrow][ncol] = true;
                    q.push({dist + 1, {nrow, ncol}});
                }
            }
        }

        // If we exhaust the queue without finding the destination
        return -1;
    }
};

```

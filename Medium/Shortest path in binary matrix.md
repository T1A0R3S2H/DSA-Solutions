### Code
```c
class Solution {
public:
    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        int n=grid.size();
        int drow[]={-1, -1, 0, 1, 1, 1, 0, -1};
        int dcol[]={0, 1, 1, 1, 0, -1, -1, -1};

        if(grid[0][0]==1) return -1;

        queue<pair<int, pair<int, int>>>pq;
        pq.push({0, {0, 0}});
        vector<vector<int>> dist(n, vector<int>(n, 1e9));
        dist[0][0]=0;

        while(!pq.empty()){
            auto it = pq.front();
            pq.pop();
            int wt=it.first;
            int row=it.second.first;
            int col=it.second.second;

            if(row==n-1 && col==n-1) return wt+1;

            for(int i=0;i<8;i++){
                int nrow=row+drow[i];
                int ncol=col+dcol[i];
                if(nrow>=0 && ncol>=0 && nrow<n && ncol<n && grid[nrow][ncol]==0 && dist[nrow][ncol]>1+wt){
                    dist[nrow][ncol]=wt+1;
                    pq.push({wt+1,{nrow,ncol}});
                }
            }
        }
        return -1;
    }
};
```

### Explanation

The task is to find the shortest clear path in an `n x n` binary matrix from the top-left cell `(0, 0)` to the bottom-right cell `(n-1, n-1)`. A clear path consists of cells with value `0`, and moves are allowed in 8 directions (horizontally, vertically, and diagonally).

1. **Initialization**:
   - **Direction Vectors**: Define 8 possible directions for movement.
   - **Priority Queue**: Use a priority queue to store cells with their current distance from the start.
   - **Distance Matrix**: Keep track of the shortest distance to each cell, initialized to a large value (`1e9`), except the starting cell which is `0`.

2. **BFS Traversal**:
   - Start from the top-left cell.
   - For each cell, explore all 8 directions.
   - Update the distance and add cells to the queue if a shorter path is found.
   - If the bottom-right cell is reached, return the distance plus one.

3. **Termination**:
   - If the queue is empty and the bottom-right cell has not been reached, return `-1` indicating no path exists.

### Time Complexity

- **BFS Complexity**: Each cell is processed once.
- **Priority Queue Operations**: Each cell can be added to the priority queue at most once, and each insertion takes \(O(\log N)\) time.
- **Overall Time Complexity**: \(O(N^2 \log N)\), where \(N\) is the size of the matrix (i.e., \(n \times n\)).

### Space Complexity

- **Distance Matrix**: \(O(N^2)\) to store the distances of all cells.
- **Priority Queue**: At most \(O(N^2)\) entries.
- **Overall Space Complexity**: \(O(N^2)\).

### Dry Run

For the input `grid = [[0, 0, 0], [1, 1, 0], [1, 1, 0]]`:

1. **Initialization**:
   - `pq` (priority queue): `[(0, (0, 0))]` (Start cell with distance 0)
   - `dist`: `[[0, 1e9, 1e9], [1e9, 1e9, 1e9], [1e9, 1e9, 1e9]]`

2. **Processing `(0, 0)`**:
   - Current distance `wt = 0`
   - Explore directions:
     - Right `(0, 1)` → Update `dist[0][1] = 1`, add to `pq`
     - Down `(1, 0)` → Blocked
     - Diagonal `(1, 1)` → Blocked
     - Other directions are out of bounds.

3. **Processing `(0, 1)`**:
   - Current distance `wt = 1`
   - Explore directions:
     - Right `(0, 2)` → Update `dist[0][2] = 2`, add to `pq`
     - Down `(1, 1)` → Blocked
     - Diagonal `(1, 2)` → Update `dist[1][2] = 2`, add to `pq`

4. **Processing `(0, 2)`**:
   - Current distance `wt = 2`
   - Explore directions:
     - Down `(1, 2)` → Already processed
     - Diagonal `(2, 2)` → Update `dist[2][2] = 3`, add to `pq`

5. **Processing `(1, 2)`**:
   - Current distance `wt = 2`
   - Explore directions:
     - Diagonal `(2, 2)` → Already processed
     - Other directions are blocked or out of bounds.

6. **Processing `(2, 2)`**:
   - Current distance `wt = 3`
   - Reached destination `(2, 2)` with a path length of `4`.

### Result

The shortest path length is `4`.

**Path**: `(0, 0) → (0, 1) → (1, 2) → (2, 2)`

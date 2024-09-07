### Code:
```cpp
class Solution {
public:
    int orangesRotting(vector<vector<int>>& grid) {
        int m=grid.size();
        int n=grid[0].size();
        queue<pair<int, pair<int, int>>> q;  // {minutes, {row, col}}
        int freshCount=0;

        // Directions for moving up, down, left, right
        vector<pair<int, int>> directions={{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
        
        // Push all initially rotten oranges into the queue
        for (int i=0; i<m; i++) {
            for (int j=0; j<n; j++) {
                if (grid[i][j]==2) {
                    q.push({0, {i, j}});  // Push rotten orange with 0 minutes
                } 
                else if (grid[i][j] == 1) {
                    freshCount++;
                }
            }
        }
        
        if (freshCount==0) return 0;
        int minutes=0;
        // BFS
        while (!q.empty()) {
            int time=q.front().first;
            int row=q.front().second.first;
            int col=q.front().second.second;
            q.pop();
            minutes=time;  // Track the last time it took to rot

            for (auto dir:directions) {
                int newRow=row+dir.first;
                int newCol=col+dir.second;
                
                // Check if the new position is valid and contains a fresh orange
                if (newRow>=0 && newRow<m && newCol>=0 && newCol<n && grid[newRow][newCol]==1){
                    grid[newRow][newCol]=2;  // Rot the fresh orange
                    freshCount--;            // Decrease fresh count
                    q.push({time + 1, {newRow, newCol}});  // Push with incremented time
                }
            }
        }
        
        // If there are still fresh oranges left, return -1
        return freshCount==0?minutes:-1;
    }
};
```
### Explanation:
1. **Initialization**:
   - `m` and `n` represent the number of rows and columns of the grid.
   - `queue<pair<int, pair<int, int>>> q` stores the rotten oranges in the format `{minutes, {row, col}}`.
   - `freshCount` keeps track of the number of fresh oranges in the grid.
   - The `directions` vector stores the 4 possible directions (up, down, left, right) for rotting adjacent oranges.

2. **Filling the Queue**:
   - The grid is scanned, and all rotten oranges are pushed into the queue with an initial minute count of 0.
   - Fresh oranges are counted using `freshCount`.

3. **Early Exit**:
   - If there are no fresh oranges (`freshCount == 0`), the function immediately returns `0` because no time is needed to rot oranges.

4. **BFS (Breadth-First Search)**:
   - For each rotten orange, the program checks its 4 neighbors.
   - If a neighbor is a fresh orange (`grid[newRow][newCol] == 1`), it becomes rotten (`grid[newRow][newCol] = 2`), and the neighbor is pushed into the queue with an incremented time.
   - The `minutes` variable is updated with the last rotten time.
   - The `freshCount` decreases every time a fresh orange becomes rotten.

5. **Final Check**:
   - If there are no fresh oranges left after the BFS, the function returns the total number of minutes. If any fresh oranges remain, the function returns `-1`.

### Time Complexity:
- **O(m * n)**: Each cell in the grid is visited once during the BFS.
  - Both the scanning of the grid and the BFS take `O(m * n)` time.
  - The queue stores at most `m * n` elements, making queue operations efficient.

### Space Complexity:
- **O(m * n)**: The space is used by the queue and auxiliary vectors like `directions`. Since the worst-case scenario involves pushing every cell into the queue, the space complexity is proportional to the number of grid cells.

### Dry Run:

#### Input:
```plaintext
grid = [
    [2, 1, 1],
    [1, 1, 0],
    [0, 1, 1]
]
```

#### Initial State:
- `m = 3`, `n = 3`
- `freshCount = 5` (since there are 5 fresh oranges).
- Initial queue (`q`) contains: `{0, {0, 0}}` (only the rotten orange at position `(0, 0)`).

#### BFS Process:

1. **Minute 0**:
   - Process `{0, {0, 0}}` (rotten orange at position `(0, 0)`).
   - Check its neighbors:
     - Right neighbor `(0, 1)` becomes rotten: Push `{1, {0, 1}}`.
     - Down neighbor `(1, 0)` becomes rotten: Push `{1, {1, 0}}`.
   - `freshCount = 3`.

2. **Minute 1**:
   - Process `{1, {0, 1}}` (rotten orange at position `(0, 1)`).
   - Check its neighbors:
     - Right neighbor `(0, 2)` becomes rotten: Push `{2, {0, 2}}`.
     - Down neighbor `(1, 1)` becomes rotten: Push `{2, {1, 1}}`.
   - `freshCount = 1`.

   - Process `{1, {1, 0}}` (rotten orange at position `(1, 0)`).
   - Check its neighbors (no fresh orange).

3. **Minute 2**:
   - Process `{2, {0, 2}}` (rotten orange at position `(0, 2)`).
   - Check its neighbors (no fresh orange).
   
   - Process `{2, {1, 1}}` (rotten orange at position `(1, 1)`).
   - Check its neighbors:
     - Down neighbor `(2, 1)` becomes rotten: Push `{3, {2, 1}}`.
   - `freshCount = 0`.

4. **Minute 3**:
   - Process `{3, {2, 1}}` (rotten orange at position `(2, 1)`).
   - Check its neighbors:
     - Right neighbor `(2, 2)` becomes rotten: Push `{4, {2, 2}}`.
   - `freshCount = 0`.

5. **Minute 4**:
   - Process `{4, {2, 2}}` (last orange).
   - No fresh oranges left.

#### Final State:
- `freshCount = 0`, and the last minute was `4`.

#### Output:
```plaintext
4
```

The minimum number of minutes required to rot all oranges is `4`.

---

This dry run matches the first example you provided. Let me know if you need more details!

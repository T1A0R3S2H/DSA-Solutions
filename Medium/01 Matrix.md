### Code
```cpp
class Solution {
public:
    vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
        queue<pair<pair<int, int>, int>> q;

        int n=mat.size();
        int m=mat[0].size();
        vector<vector<int>>visited(n,vector<int>(m,0));
        vector<vector<int>>ans(n,vector<int>(m,0));
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(mat[i][j]==0){
                    q.push({{i,j},0});
                    ans[i][j]=0;
                    visited[i][j]=1;
                }
            }
        }

        while(!q.empty()){
            int i=q.front().first.first;
            int j=q.front().first.second;
            int dis=q.front().second;
            q.pop();
            int arrrow[]={-1,0,1,0};
            int arrcol[]={0,-1,0,1};
            int x=0;
            while(x<4){
                int ni=i+arrrow[x];
                int nj=j+arrcol[x];
                if(ni>=0 && nj>=0 && ni<n && nj<m){
                if(!visited[ni][nj]){
                    visited[ni][nj]++;
                    ans[ni][nj]=dis+1;
                    q.push({{ni,nj},dis+1});
                }}
                x++;
            }
        }
        return ans;
    }
};
```
### Explanation

The problem is to find the distance of each cell in a binary matrix from the nearest cell containing a `0`. The solution uses a Breadth-First Search (BFS) approach starting from all cells containing `0`. This ensures that we process each cell in the shortest possible distance from a `0`.

### Approach

1. **Initialization:**
   - Initialize a queue `q` to perform BFS.
   - Initialize a `visited` matrix to keep track of cells that have already been processed.
   - Initialize an `ans` matrix to store the distance from the nearest `0` for each cell.

2. **Queue Initialization:**
   - Iterate through the matrix and for each cell containing `0`, push it into the queue with a distance of `0`. Also mark these cells as visited in the `visited` matrix.

3. **BFS Traversal:**
   - While the queue is not empty:
     - Dequeue the front element to get the current cell's coordinates and distance.
     - For each of the four possible directions (up, down, left, right), calculate the new cell coordinates.
     - If the new cell is within bounds and not yet visited:
       - Update the `ans` matrix with the new distance.
       - Mark the cell as visited and enqueue it with the updated distance.

4. **Return the `ans` matrix** which contains the shortest distance of each cell from the nearest `0`.

### Time Complexity

The time complexity of the BFS approach is \(O(m \times n)\), where \(m\) is the number of rows and \(n\) is the number of columns. This is because each cell is enqueued and dequeued exactly once.

### Space Complexity

The space complexity is also \(O(m \times n)\). This includes:
- The `visited` matrix of size \(m \times n\).
- The `ans` matrix of size \(m \times n\).
- The queue, which can hold up to \(m \times n\) elements in the worst case.

### Dry Run

**Input:**
```cpp
mat = [[0,0,0],[0,1,0],[1,1,1]]
```

**Process:**

1. Initialize the `visited` and `ans` matrices:
   ```cpp
   visited = [[0,0,0],[0,0,0],[0,0,0]]
   ans = [[0,0,0],[0,0,0],[0,0,0]]
   ```

2. Initialize the queue with cells containing `0`:
   ```cpp
   q = [({0,0},0), ({0,1},0), ({0,2},0), ({1,0},0), ({2,0},1), ({1,2},0), ({2,1},1), ({2,2},1)]
   ```

3. Process each cell from the queue:
   - For cell `(0,0)` with distance `0`:
     - Update neighbors `(1,0)` and `(0,1)`:
       ```cpp
       ans = [[0,0,0],[1,0,0],[1,0,0]]
       q = [({0,1},0), ({1,0},0), ({2,0},1), ({1,2},0), ({2,1},1), ({2,2},1), ({1,1},1)]
       ```
   - For cell `(0,1)` with distance `0`:
     - Update neighbors `(0,2)`:
       ```cpp
       ans = [[0,0,0],[1,0,0],[1,1,0]]
       q = [({1,0},0), ({2,0},1), ({1,2},0), ({2,1},1), ({2,2},1), ({1,1},1)]
       ```
   - Continue processing until all cells are processed.

**Output:**
```cpp
ans = [[0,0,0],[0,1,0],[1,2,1]]
```

This output indicates the shortest distance from each cell to the nearest `0`.

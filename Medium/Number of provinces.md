### Code:
```cpp
class Solution {
public:
    void dfs(vector<vector<int>>& arr, int curr, vector<bool>&visited){
        visited[curr]=true;
        for(int i=0;i<arr[curr].size();i++){
            if(arr[curr][i]==1 && !visited[i]){
                dfs(arr, i, visited);
            }
        }
    }
    int findCircleNum(vector<vector<int>>& isConnected) {
        int n=isConnected.size();
        vector<bool> visited(n, false);
        int count=0;
        for(int i=0;i<isConnected.size();i++){
            if(!visited[i]){
                count++;
                dfs(isConnected, i, visited);
            }
        }
        return count;
    }
};
```
### **Explanation:**

The problem can be understood as finding the number of **connected components** (provinces) in an undirected graph, where cities are nodes and connections between them are edges. This graph is represented by an adjacency matrix, `isConnected`, where:
- `isConnected[i][j] = 1` means city `i` is directly connected to city `j`.
- `isConnected[i][j] = 0` means there is no direct connection.

We can solve this using Depth-First Search (DFS):
1. **DFS Traversal**: We start from any unvisited city, mark it as visited, and recursively visit all cities directly or indirectly connected to it.
2. **Counting Provinces**: Each time we start a DFS from an unvisited city, it represents a new province (connected component), and we increase the count.
3. **Visited Array**: To keep track of visited cities, we use a `visited` array, initialized to `false` for all cities.

### **Time Complexity:**
- **O(n²)**: The time complexity is based on traversing the entire adjacency matrix `isConnected`, which is of size `n x n` where `n` is the number of cities.
  - In the DFS, for each city, we visit all its neighbors in the worst case, leading to a quadratic time complexity.

### **Space Complexity:**
- **O(n)**: We use a `visited` array of size `n` to track visited cities and the recursion stack in DFS, which in the worst case could be as deep as `n`.

### **Dry Run:**

Let’s dry run the code with **Example 1**:

#### Input:
```cpp
isConnected = [[1,1,0],
               [1,1,0],
               [0,0,1]]
```

#### Step-by-step Dry Run:
- **Initialization**: 
  - `n = 3`, `visited = [false, false, false]`, `count = 0`
  
- **Iteration 1 (`i = 0`)**:
  - Since `visited[0] == false`, we start a new DFS from city `0`:
    - **DFS(0)**: Mark `visited[0] = true`. Then, check neighbors of city 0:
      - `isConnected[0][1] == 1 && !visited[1]`, so we recursively call `DFS(1)`:
        - **DFS(1)**: Mark `visited[1] = true`. Check neighbors of city 1:
          - `isConnected[1][0] == 1 && visited[0] == true` (already visited).
          - `isConnected[1][1] == 1 && visited[1] == true` (self-loop).
          - `isConnected[1][2] == 0` (no connection).
        - Return from DFS(1).
      - `isConnected[0][2] == 0` (no connection).
    - Return from DFS(0).
  - After DFS(0), one province is found (`count = 1`).

- **Iteration 2 (`i = 1`)**:
  - `visited[1] == true`, so skip DFS.

- **Iteration 3 (`i = 2`)**:
  - Since `visited[2] == false`, we start a new DFS from city `2`:
    - **DFS(2)**: Mark `visited[2] = true`. Then, check neighbors of city 2:
      - `isConnected[2][0] == 0` (no connection).
      - `isConnected[2][1] == 0` (no connection).
      - `isConnected[2][2] == 1 && visited[2] == true` (self-loop).
    - Return from DFS(2).
  - After DFS(2), another province is found (`count = 2`).

#### Final Output:
- After all iterations, we have `count = 2` provinces.

### Final Thoughts:
- This approach efficiently finds the number of connected components (provinces) in the graph using DFS and tracks visited nodes with an array.

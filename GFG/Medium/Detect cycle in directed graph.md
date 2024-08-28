### Code
```cpp
class Solution {
  public:
    // Function to detect cycle in a directed graph.
    bool isCyclic(int V, vector<int> adj[]) {
        vector<int> visited(V, 0);
        vector<int> dfsVisited(V, 0);
        for(int i=0; i<V; i++) {
            if(!visited[i]) {
                if(dfs(i, visited, dfsVisited, adj)) {
                    return true;
                }
            }
        }
        return false;
    }
    
    bool dfs(int node, vector<int>& visited, vector<int>& dfsVisited, vector<int> adj[]) {
        visited[node]=1;
        dfsVisited[node]=1;
        
        for(auto& it:adj[node]) {
            if(!visited[it]) {
                if(dfs(it, visited, dfsVisited, adj)) {
                    return true;
                }
            } 
            else if(dfsVisited[it]&&visited[it]) {
                return true;
            }
        }
        
        dfsVisited[node]=0; // Unmark the node when backtracking
        return false;
    }
};
```
### Explanation

The code aims to detect a cycle in a directed graph using a Depth-First Search (DFS) approach. It maintains two vectors:

1. **`visited[]`**: This keeps track of all nodes that have been visited at least once.
2. **`dfsVisited[]`**: This keeps track of nodes that are currently in the current DFS call stack. It's used to detect back edges, which indicate cycles.

#### Function Breakdown

1. **`isCyclic()`**:
   - It initializes two vectors, `visited` and `dfsVisited`, both of size `V` (the number of vertices) and initializes them to 0.
   - It iterates through all vertices and, if a vertex hasn't been visited, it calls the `dfs()` function.
   - If `dfs()` returns `true` for any vertex, it means a cycle is detected, and the function returns `true`.
   - If no cycle is detected in any DFS traversal, it returns `false`.

2. **`dfs()`**:
   - This function performs the DFS traversal.
   - It marks the current node as visited in both `visited` and `dfsVisited`.
   - It iterates over all adjacent nodes:
     - If an adjacent node hasn't been visited, it recursively calls `dfs()` on that node.
     - If an adjacent node is already in the `dfsVisited` (i.e., part of the current path), it indicates a cycle, and the function returns `true`.
   - Once done with all adjacent nodes, it unmarks the current node from `dfsVisited` (backtracking) and returns `false`.

### Time Complexity Analysis

- **Time Complexity**: \(O(V + E)\)
  - Each vertex is processed once, and each edge is checked once in the adjacency list. Thus, the time complexity is linear in terms of the number of vertices \(V\) and edges \(E\).

### Space Complexity Analysis

- **Space Complexity**: \(O(V)\)
  - Two vectors of size \(V\) are used (`visited` and `dfsVisited`), resulting in an \(O(V)\) space complexity.
  - The recursion stack space in the worst case could go up to \(O(V)\) as well, but this does not affect the overall complexity.

### Dry Run

Let's perform a dry run with an example graph:

**Graph**:
- Vertices: 4 (0, 1, 2, 3)
- Edges: [(0, 1), (1, 2), (2, 0), (2, 3)]

**Adjacency List Representation**:
- `adj[0] = {1}`
- `adj[1] = {2}`
- `adj[2] = {0, 3}`
- `adj[3] = {}`

**Execution Flow**:

1. `isCyclic(4, adj)` is called.
2. Initializes `visited = [0, 0, 0, 0]` and `dfsVisited = [0, 0, 0, 0]`.
3. Iterates over each node:
   - **Node 0**:
     - Not visited, calls `dfs(0, visited, dfsVisited, adj)`.
     - Marks `visited[0] = 1`, `dfsVisited[0] = 1`.
     - Iterates over `adj[0] = {1}`:
       - **Node 1**:
         - Not visited, calls `dfs(1, visited, dfsVisited, adj)`.
         - Marks `visited[1] = 1`, `dfsVisited[1] = 1`.
         - Iterates over `adj[1] = {2}`:
           - **Node 2**:
             - Not visited, calls `dfs(2, visited, dfsVisited, adj)`.
             - Marks `visited[2] = 1`, `dfsVisited[2] = 1`.
             - Iterates over `adj[2] = {0, 3}`:
               - **Node 0** (Back to start, forms a cycle):
                 - Already visited and `dfsVisited[0] = 1` (indicates cycle).
                 - Returns `true` from `dfs(2)`.
             - Returns `true` from `dfs(1)`.
         - Returns `true` from `dfs(0)`.
   - **Node 0** cycle detected, so `isCyclic` returns `true`.

### Conclusion

The code correctly identifies the presence of a cycle in the directed graph using DFS and backtracking. The `visited` and `dfsVisited` vectors play crucial roles in tracking the state of nodes to identify cycles efficiently. The dry run confirms the functionality and accuracy of the approach.

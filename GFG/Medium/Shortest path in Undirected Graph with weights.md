### Code:
```c
vector<int> shortestPath(vector<vector<int>>& edges, int N, int M, int src) {
        // Create the adjacency list
        unordered_map<int, list<int>> adj;
        for(int i=0; i<M; i++) {
            int u=edges[i][0];
            int v=edges[i][1];
            adj[u].push_back(v);
            adj[v].push_back(u);
        }
        vector<int> distance(N, -1);
        // BFS initialization
        queue<int> q;
        distance[src]=0;  // Distance to the source is 0
        q.push(src);
        while(!q.empty()) {
            int node=q.front();
            q.pop();
            
            // Traverse all adjacent nodes of the current node
            for(auto it:adj[node]) {
                if(distance[it]==-1) {  // If the neighbor has not been visited
                    distance[it]=distance[node] + 1;  // Update its distance
                    q.push(it);  // Push the neighbor into the queue for further exploration
                }
            }
        }
        return distance;
    }
```

Let's go through the **ETSD (Explanation, Time Complexity, Space Complexity, and Dry Run)** of your code.

### **Explanation**:
1. **Graph Representation**: The graph is represented using an adjacency list, where each node points to a list of its connected neighbors.
2. **BFS Algorithm**: Since the graph has unit weight edges, BFS is the ideal choice to find the shortest path. BFS guarantees the shortest path when the edge weights are uniform.
3. **Distance Array**: The `distance` array is initialized to `-1`, representing unreachable nodes. The distance to the `src` node is set to `0` since the shortest path from a node to itself is zero.
4. **Queue**: A queue is used to explore the graph layer by layer, starting from the source. Each node's neighbors are checked, and their distance is updated the first time they are visited.
5. **Loop**: The loop continues until all reachable nodes are processed. Unreachable nodes retain their `-1` values in the distance array.

### **Time Complexity**:
- **O(N + M)**, where `N` is the number of nodes and `M` is the number of edges. 
  - Constructing the adjacency list takes **O(M)** time.
  - BFS processes every node and edge once, taking **O(N + M)**.

### **Space Complexity**:
- **O(N + M)**:
  - The adjacency list takes **O(N + M)** space.
  - The `distance` array takes **O(N)** space.
  - The queue in BFS can hold up to **O(N)** nodes at once in the worst case.

### **Dry Run**:

#### Input:
```cpp
n = 4 (0, 1, 2, 3), m = 2 (edges)
edges = {{1, 3}, {3, 0}}
src = 3
```

#### Step-by-Step Execution:

1. **Graph Construction** (Adjacency List):
   - 0 → {3}
   - 1 → {3}
   - 3 → {0, 1}

2. **BFS Execution**:
   - Initialize `distance = [-1, -1, -1, 0]` and start from `src = 3`.
   - `queue = {3}`:
     - Pop node `3`, visit its neighbors:
       - Node `0`: Update `distance[0] = 1` and enqueue `0`.
       - Node `1`: Update `distance[1] = 1` and enqueue `1`.
   - `queue = {0, 1}`:
     - Pop node `0`, but its only neighbor is `3`, which has already been visited.
     - Pop node `1`, but its only neighbor is `3`, which has already been visited.

3. **Final Distance Array**: 
   - `distance = {1, 1, -1, 0}`

#### Output:
```cpp
1 1 -1 0
```

The shortest paths are correctly computed, and unreachable nodes are marked as `-1`.

Let me know if you need further clarification or modifications!

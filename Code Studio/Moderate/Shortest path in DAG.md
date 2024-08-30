### Code
```cpp
#include<bits/stdc++.h>
void toposort(int node, vector<pair<int, int>> adj[], vector<int>& vis, stack<int>& st) {
    vis[node] = 1;
    for (auto it : adj[node]) {
        if (!vis[it.first]) { // Check it.first instead of it
            toposort(it.first, adj, vis, st);
        }
    }
    st.push(node);
}

 

vector<int> shortestPathInDAG(int n, int m, vector<vector<int>> &edges) {
// Creating graph

    vector<pair<int, int>> adj[n];
    for (int i=0; i<m; i++) {
        int u=edges[i][0];
        int v=edges[i][1];
        int wt=edges[i][2];

        adj[u].push_back({v, wt});
    }

 

// Topological sorting
stack<int> st;
vector<int> vis(n, 0);

// Perform topological sort for all components
for (int i=0; i<n; i++) {
    if (!vis[i]) {
        toposort(i, adj, vis, st);
    }

}

// Initialize distance vector to store the shortest path distances
vector<int> dist(n, 1e9);

// Assuming the source node is 0 (you may modify it as per your requirement)
dist[0]=0;

// Process nodes in topological order
while (!st.empty()) {
    int node = st.top();
    st.pop();
    if (dist[node] != 1e9) { // Only proceed if the current node has been reached
        for (auto it : adj[node]) {
            int v = it.first;
            int wt = it.second;
            if (dist[node]+wt < dist[v]) {
                dist[v]=dist[node]+wt;
            }
        }
    }

}

// Replace unreachable nodes' distances with -1
for (int i = 0; i < n; i++) {
    if (dist[i]==1e9) dist[i]=-1;
}

return dist;

}
```
### Explanation

1. **Graph Representation**:
   - The graph is represented using an adjacency list where each node has a list of pairs. Each pair consists of a connected node and the weight of the edge connecting them.
   - The adjacency list is created from the input `edges`, which contains the list of edges with their weights.

2. **Topological Sorting**:
   - A topological sort is performed using Depth-First Search (DFS). Nodes are pushed onto a stack to ensure they are processed in topological order.
   - The `toposort` function is a recursive DFS function that marks nodes as visited and pushes them onto the stack once all their descendants are visited.

3. **Shortest Path Calculation**:
   - After obtaining the topological order, we initialize a distance vector with a large value (`1e9`) to represent infinity.
   - Assuming the source node is `0`, its distance is initialized to `0`.
   - Nodes are processed in topological order. For each node, the algorithm updates the distance of its adjacent nodes if a shorter path is found using the current node.

4. **Handling Unreachable Nodes**:
   - Nodes that remain with a distance of `1e9` are considered unreachable from the source node, and their distances are replaced with `-1`.

### Time Complexity

- **Graph Creation**: `O(m)` - where `m` is the number of edges. Each edge is processed once to create the adjacency list.
- **Topological Sorting**: `O(n + m)` - where `n` is the number of nodes and `m` is the number of edges. Each node and edge is processed once in DFS.
- **Shortest Path Calculation**: `O(n + m)` - Each node is processed in topological order, and each edge is checked once.
- **Overall Complexity**: `O(n + m)` - This is efficient for DAGs since we process each node and edge once.

### Space Complexity

- **Adjacency List**: `O(n + m)` - Storing the graph.
- **Visited Vector**: `O(n)` - To keep track of visited nodes.
- **Stack for Topological Sort**: `O(n)` - Maximum size is the number of nodes.
- **Distance Vector**: `O(n)` - Storing the shortest distances from the source.
- **Overall Space Complexity**: `O(n + m)`.

### Dry Run

Let's dry run the code with an example.

#### Input

- `n = 6` (number of nodes)
- `m = 7` (number of edges)
- `edges = [[0, 1, 2], [0, 4, 1], [1, 2, 3], [4, 2, 2], [4, 5, 4], [2, 3, 6], [5, 3, 1]]`

#### Graph Representation

- `adj[0] = [(1, 2), (4, 1)]`
- `adj[1] = [(2, 3)]`
- `adj[2] = [(3, 6)]`
- `adj[4] = [(2, 2), (5, 4)]`
- `adj[5] = [(3, 1)]`

#### Topological Sorting

Using DFS starting from node `0`:

- Visit `0`, then `1`, then `2`, then `3` (push `3` onto stack), backtrack and push `2` and `1`, back to `0`, visit `4`, then `5`, push `3` again but it's already visited, then push `5` and `4`, finally push `0`.

Topological order in stack: `[0, 4, 5, 1, 2, 3]`

#### Shortest Path Calculation

- Initialize distances: `dist = [0, 1e9, 1e9, 1e9, 1e9, 1e9]`

Process in topological order:

1. Node `0`:
   - Update `dist[1] = 2`, `dist[4] = 1` 
   - `dist = [0, 2, 1e9, 1e9, 1, 1e9]`
   
2. Node `4`:
   - Update `dist[2] = 3`, `dist[5] = 5`
   - `dist = [0, 2, 3, 1e9, 1, 5]`

3. Node `5`:
   - Update `dist[3] = 6`
   - `dist = [0, 2, 3, 6, 1, 5]`

4. Node `1`:
   - Update `dist[2]` is not changed since `2 + 3 = 5` which is greater than `dist[2] = 3`

5. Node `2`:
   - Update `dist[3]` is not changed since `3 + 6 = 9` which is greater than `dist[3] = 6`

6. Node `3`:
   - No outgoing edges to process

#### Final Distances

- `dist = [0, 2, 3, 6, 1, 5]`

### Conclusion

The code correctly calculates the shortest paths from the source node to all other nodes in the DAG. The distances represent the minimum cost to reach each node from the source node `0`. Unreachable nodes would be marked with `-1` after the replacement step.

### Code
```cpp
class Solution {
public:
    void toposort(int node, vector<pair<int, int>> adj[], vector<int>& vis, stack<int>& st) {
        vis[node] = 1;
        for (auto it : adj[node]) {
            if (!vis[it.first]) {
                toposort(it.first, adj, vis, st);
            }
        }
        st.push(node);
    }

    vector<int> shortestPath(int N, int M, vector<vector<int>>& edges) {
        // Creating graph
        vector<pair<int, int>> adj[N];
        for (int i = 0; i < M; i++) {
            int u = edges[i][0];
            int v = edges[i][1];
            int wt = edges[i][2];
            adj[u].push_back({v, wt});
        }

        // Topological sorting
        stack<int> st;
        vector<int> vis(N, 0);

        // Perform topological sort for all components
        for (int i = 0; i < N; i++) {
            if (!vis[i]) {
                toposort(i, adj, vis, st);
            }
        }

        // Initialize distance vector to store the shortest path distances
        vector<int> dist(N, 1e9);

        // Assuming the source node is 0 (you may modify it as per your requirement)
        dist[0] = 0;

        // Process nodes in topological order
        while (!st.empty()) {
            int node = st.top();
            st.pop();
            if (dist[node] != 1e9) { // Only proceed if the current node has been reached
                for (auto it : adj[node]) {
                    int v = it.first;
                    int wt = it.second;
                    if (dist[node] + wt < dist[v]) {
                        dist[v] = dist[node] + wt;
                    }
                }
            }
        }

        // Replace unreachable nodes' distances with -1
        for (int i = 0; i < N; i++) {
            if (dist[i] == 1e9) dist[i] = -1;
        }

        return dist;
    }
};
```

### Explanation

1. **Graph Representation**:
   - The function takes `N` (number of nodes), `M` (number of edges), and a list of `edges` as input.
   - The graph is represented using an adjacency list `adj`, where each node points to a list of pairs. Each pair consists of a connected node and the weight of the edge connecting them.

2. **Topological Sorting**:
   - A helper function `toposort` performs a Depth-First Search (DFS) to achieve topological sorting. 
   - Nodes are marked as visited using the `vis` vector, and once all the descendants of a node are processed, the node is pushed onto a stack (`st`).
   - The stack ensures that nodes are processed in topological order when computing the shortest path.

3. **Shortest Path Calculation**:
   - A distance vector `dist` is initialized with a large value (`1e9`), representing infinity. This indicates that initially, all nodes are unreachable.
   - The source node is assumed to be `0` (this can be modified if needed), and its distance is set to `0`.
   - Nodes are then processed in topological order using the stack. For each node, the algorithm updates the distance of its adjacent nodes if a shorter path is found through the current node.

4. **Handling Unreachable Nodes**:
   - After processing, any node that still has a distance of `1e9` is considered unreachable from the source node, and its distance is replaced with `-1`.

### Time Complexity Analysis

- **Graph Creation**: `O(M)` - Each edge is processed once to create the adjacency list.
- **Topological Sorting**: `O(N + M)` - Each node and edge is processed once using DFS.
- **Shortest Path Calculation**: `O(N + M)` - Each node is processed in topological order, and each edge is checked once.
- **Overall Complexity**: `O(N + M)` - This is efficient for DAGs since we process each node and edge once.

### Space Complexity Analysis

- **Adjacency List**: `O(N + M)` - Storing the graph with `N` nodes and `M` edges.
- **Visited Vector**: `O(N)` - To keep track of visited nodes.
- **Stack for Topological Sort**: `O(N)` - The maximum size of the stack is the number of nodes.
- **Distance Vector**: `O(N)` - Storing the shortest distances from the source.
- **Overall Space Complexity**: `O(N + M)`.

### Dry Run

Let's perform a dry run of the code with an example input.

#### Input

- `N = 6` (number of nodes)
- `M = 7` (number of edges)
- `edges = [[0, 1, 2], [0, 4, 1], [1, 2, 3], [4, 2, 2], [4, 5, 4], [2, 3, 6], [5, 3, 1]]`

#### Graph Representation

Using the edges, the adjacency list (`adj`) will look like this:

- `adj[0] = [(1, 2), (4, 1)]`
- `adj[1] = [(2, 3)]`
- `adj[2] = [(3, 6)]`
- `adj[4] = [(2, 2), (5, 4)]`
- `adj[5] = [(3, 1)]`
- `adj[3] = []` (No outgoing edges from node 3)

#### Topological Sorting

Performing DFS to achieve topological sort:

1. Start from node `0`:
   - Visit `0`, then visit `1`, then `2`, then `3`.
   - Push `3` onto the stack, backtrack, push `2`, then `1`, and finally `0`.
   - Move to the next unvisited node, which is `4`.
   - Visit `4`, then `5`, and push `5` and `4` onto the stack.

Topological order in stack: `[0, 4, 5, 1, 2, 3]`.

#### Shortest Path Calculation

1. Initialize distances: `dist = [0, 1e9, 1e9, 1e9, 1e9, 1e9]`.

Process nodes in topological order:

1. **Node `0`**:
   - Update `dist[1] = 2` (0 + 2), `dist[4] = 1` (0 + 1).
   - `dist = [0, 2, 1e9, 1e9, 1, 1e9]`.

2. **Node `4`**:
   - Update `dist[2] = 3` (1 + 2), `dist[5] = 5` (1 + 4).
   - `dist = [0, 2, 3, 1e9, 1, 5]`.

3. **Node `5`**:
   - Update `dist[3] = 6` (5 + 1).
   - `dist = [0, 2, 3, 6, 1, 5]`.

4. **Node `1`**:
   - No update to `dist[2]` since `2 + 3 = 5` is greater than `dist[2] = 3`.

5. **Node `2`**:
   - No update to `dist[3]` since `3 + 6 = 9` is greater than `dist[3] = 6`.

6. **Node `3`**:
   - No outgoing edges to process.

#### Final Distances

- `dist = [0, 2, 3, 6, 1, 5]`

### Conclusion

The code successfully calculates the shortest paths from the source node `0` to all other nodes in the DAG. The distances represent the minimum cost to reach each node from the source. Nodes that remain unreachable from the source have their distances marked as `-1`. This approach leverages the topological order to efficiently calculate the shortest paths, which is suitable for DAGs.

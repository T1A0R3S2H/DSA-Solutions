### Prim's Algorithm Code for Minimum Spanning Tree

#### Assumption:
The implementation of Prim's algorithm assumes the graph is an undirected, connected, and weighted graph with non-negative edge weights, where vertices are numbered from 1 to `n`. It also assumes the input graph is provided as a vector of edges, and there are no self-loops or multiple edges between the same pair of vertices. The algorithm starts constructing the MST from vertex 1 and expects the graph to be connected, ensuring all vertices are reachable.

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<pair<pair<int, int>, int>> calculatePrimsMST(int n, int m, vector<pair<pair<int, int>, int>> &g) {
    // Adjacency list to represent the graph
    unordered_map<int, list<pair<int, int>>> adj;

    // Building the adjacency list from the given edges
    for (int i = 0; i < g.size(); i++) {
        int u = g[i].first.first;  // Start node of the edge
        int v = g[i].first.second; // End node of the edge
        int w = g[i].second;       // Weight of the edge
        adj[u].push_back(make_pair(v, w)); // Add edge u -> v
        adj[v].push_back(make_pair(u, w)); // Add edge v -> u (since the graph is undirected)
    }

    // Key values used to pick minimum weight edge in cut
    vector<int> key(n + 1, INT_MAX);  // Initialize all keys to infinity
    vector<int> mst(n + 1, false);    // To track vertices included in MST
    vector<int> parent(n + 1, -1);    // Array to store constructed MST

    key[1] = 0; // Start with the first vertex, set key value to 0

    // MST will have n-1 edges, so we run the loop n-1 times
    for (int i = 1; i < n; i++) {
        int mini = INT_MAX;
        int u;

        // Find the vertex with the minimum key value, which is not yet included in MST
        for (int v = 1; v <= n; v++) {
            if (key[v] < mini && mst[v] == false) {
                mini = key[v];
                u = v;
            }
        }

        mst[u] = true; // Include vertex u in MST

        // Update key values and parent index of the adjacent vertices of the picked vertex
        for (auto neighbour : adj[u]) {
            int v = neighbour.first; // Adjacent vertex
            int w = neighbour.second; // Weight of edge u-v

            // Update key only if v is not in MST and weight of u-v is smaller than current key of v
            if (mst[v] == false && w < key[v]) {
                parent[v] = u;
                key[v] = w;
            }
        }
    }

    // Construct the result MST
    vector<pair<pair<int, int>, int>> answer;
    for (int i = 2; i <= n; i++) {
        answer.push_back(make_pair(make_pair(parent[i], i), key[i]));
    }

    return answer;
}
```

### Explanation of Prim's Algorithm

1. **Initialization**:
    - The algorithm begins by building an adjacency list (`adj`) to represent the input graph, where each node points to its connected nodes and the corresponding edge weights.
    - Three vectors are initialized:
        - `key`: Stores the minimum weight edge that connects the vertex to the MST. Initialized to `INT_MAX` (infinity) for all vertices except the starting vertex (vertex 1) which is set to 0.
        - `mst`: A boolean array to keep track of vertices included in the MST. Initialized to `false`.
        - `parent`: Stores the parent node for each vertex in the MST. Initialized to `-1`.

2. **Finding Minimum Weight Edge**:
    - The algorithm iterates `n-1` times to include `n-1` edges in the MST.
    - In each iteration, it selects the vertex `u` that has the minimum key value and is not yet included in the MST (`mst[u] == false`).

3. **Updating Neighbors**:
    - For the selected vertex `u`, the algorithm examines its neighbors. If a neighbor `v` is not in the MST and the weight of the edge `u-v` is less than the current key value of `v`, it updates the `parent` and `key` arrays for `v`.

4. **Constructing MST**:
    - Once all vertices are included in the MST, the algorithm constructs the output using the `parent` array to form the MST edges.

### Time Complexity Analysis

1. **Building the Adjacency List**: This step takes O(m) time, where `m` is the number of edges since each edge is processed once.

2. **Finding the Minimum Key Vertex**: In the worst case, finding the minimum key vertex takes O(n) time for each of the `n-1` iterations, leading to O(n^2) time in total for this operation.

3. **Updating the Key and Parent Arrays**: Each vertex's adjacent vertices are processed at most once when updating the key values, taking O(m) time in total across all iterations.

Thus, the overall time complexity of this implementation is **O(n^2 + m)**, which simplifies to **O(n^2)** in a dense graph scenario where `m` can be approximated as `n^2`.

### Space Complexity Analysis

1. **Adjacency List**: The space complexity is O(n + m) since it stores `n` vertices and `m` edges.

2. **Key, MST, and Parent Arrays**: Each uses O(n) space.

3. **Total Space Complexity**: O(n + m) + O(n) = **O(n + m)**.

### Dry Run Example

Let's perform a dry run on a small graph to illustrate how the algorithm works:

#### Graph:

- Vertices: 4
- Edges:
  - (1, 2) with weight 1
  - (1, 3) with weight 4
  - (2, 3) with weight 2
  - (2, 4) with weight 3
  - (3, 4) with weight 5

#### Initialization:

- `key = [INT_MAX, 0, INT_MAX, INT_MAX, INT_MAX]`
- `mst = [false, false, false, false, false]`
- `parent = [-1, -1, -1, -1, -1]`

#### Iterations:

1. **Iteration 1**:
   - Select vertex 1 (`key[1] = 0`).
   - Include vertex 1 in MST (`mst[1] = true`).
   - Update:
     - `key[2] = 1` (1-2 edge with weight 1)
     - `key[3] = 4` (1-3 edge with weight 4)
     - `parent = [-1, -1, 1, 1, -1]`

2. **Iteration 2**:
   - Select vertex 2 (`key[2] = 1`).
   - Include vertex 2 in MST (`mst[2] = true`).
   - Update:
     - `key[3] = 2` (2-3 edge with weight 2, replacing the previous weight 4)
     - `key[4] = 3` (2-4 edge with weight 3)
     - `parent = [-1, -1, 1, 2, 2]`

3. **Iteration 3**:
   - Select vertex 3 (`key[3] = 2`).
   - Include vertex 3 in MST (`mst[3] = true`).
   - Update:
     - No updates as the neighbors already have smaller or equal weights.
     - `parent = [-1, -1, 1, 2, 2]`

4. **Iteration 4**:
   - Select vertex 4 (`key[4] = 3`).
   - Include vertex 4 in MST (`mst[4] = true`).
   - No further updates needed.

#### Final MST:

- Edges in MST:
  - (1, 2) with weight 1
  - (2, 3) with weight 2
  - (2, 4) with weight 3

These edges form the minimum spanning tree for the given graph.

This example demonstrates how Prim's algorithm efficiently constructs the MST by repeatedly selecting the minimum weight edge that connects a new vertex to the existing MST.

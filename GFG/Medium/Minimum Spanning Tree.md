# Method 1 (Prims's Algorithm)
```cpp
class Solution
{
public:
    //Function to find sum of weights of edges of the Minimum Spanning Tree.
    int spanningTree(int V, vector<vector<int>> adj[]) {
    vector<int> key(V, INT_MAX);   // Initialize key values to infinity
    vector<bool> mst(V, false); // To track vertices included in MST
    key[0] = 0;                    // Starting from vertex 0 (0-indexed)
    int totalWeight = 0;

    for (int i = 0; i < V; i++) {
        // Find the vertex with the minimum key value not in the MST
        int mini = INT_MAX, u;
        for (int j = 0; j < V; j++) {
            if (!mst[j] && key[j] < mini) {
                mini = key[j];
                u = j;
            }
        }

        // Include the chosen vertex in the MST
        mst[u] = true;
        totalWeight += key[u]; // Add the key value to total MST weight

        // Update the key values of adjacent vertices
        for (auto &neighbour : adj[u]) {
            int i = neighbour[0];
            int weight = neighbour[1];
            if (!mst[i] && weight < key[i]) {
                key[i] = weight;
            }
        }
    }

    return totalWeight; // Total weight of the MST
}
};
```
### Explanation

The given code implements Prim's algorithm to find the Minimum Spanning Tree (MST) of a connected, undirected, and weighted graph. The MST is a subset of the graph's edges that connects all the vertices together without any cycles and with the minimum possible total edge weight.

Here is the step-by-step explanation of the code:

1. **Initialization**:
   - `key`: A vector to store the minimum weight edge connecting a vertex to the MST. Initialized to `INT_MAX` for all vertices, indicating they are not yet included in the MST. `key[0]` is set to `0` to start the MST from vertex `0`.
   - `mst`: A boolean vector to track whether a vertex is included in the MST. Initialized to `false` for all vertices.
   - `totalWeight`: A variable to store the sum of the weights of the edges included in the MST.

2. **Main Loop**:
   - The loop runs `V` times (once for each vertex) to ensure all vertices are included in the MST.
   - Inside the loop, it finds the vertex `u` with the smallest key value that is not already included in the MST.
   - It then marks `u` as included in the MST by setting `mst[u] = true`.
   - The key value of `u` is added to `totalWeight`, representing the weight of the MST edge being added.

3. **Updating Adjacent Vertices**:
   - For each neighbor of vertex `u`, the code checks if the neighbor is not already included in the MST and if the edge weight is less than the current key value of the neighbor.
   - If both conditions are met, the key value is updated to the edge weight, indicating that the MST can now reach this neighbor with a smaller weight.

4. **Return Value**:
   - The function returns `totalWeight`, which represents the total weight of the edges in the MST.

### Time Complexity Analysis

- **Outer Loop**: The loop runs `V` times (for each vertex), so it contributes `O(V)`.

- **Inner Loop**: For each vertex, the code finds the vertex with the minimum key value, which takes `O(V)` time. Thus, the total time for the inner loop is `O(V^2)`.

- **Adjacency List Traversal**: The total time complexity for traversing the adjacency list of all vertices is `O(E)`, where `E` is the number of edges. However, since this traversal is inside the loop, it does not affect the overall complexity directly.

The overall time complexity of the algorithm is:

\[ O(V^2 + E) \approx O(V^2) \]

This complexity is suitable for dense graphs but can be improved to `O((V+E) \log V)` using a priority queue (min-heap).

### Space Complexity Analysis

- `key` vector: `O(V)` space.
- `mst` vector: `O(V)` space.
- Storage for the graph in the form of an adjacency list: `O(V + E)` space.

The overall space complexity is:

\[ O(V + E) \]

### Dry Run

Let's dry run the given code with a specific example to illustrate its working:

**Graph:**
```
Number of vertices (V): 3
Adjacency list:
0: [(1, 5), (2, 1)]
1: [(0, 5), (2, 3)]
2: [(0, 1), (1, 3)]
```

**Input:**
```plaintext
V = 3
Edges:
0 - 1 (weight 5)
1 - 2 (weight 3)
0 - 2 (weight 1)
```

**Dry Run Steps:**

1. **Initialization:**
   - `key = [0, INF, INF]`
   - `mst = [false, false, false]`
   - `totalWeight = 0`

2. **Iteration 1:**
   - Find minimum key: `u = 0` (key[0] = 0)
   - Include vertex `0` in MST: `mst = [true, false, false]`
   - Add key[0] to `totalWeight`: `totalWeight = 0`
   - Update keys for neighbors of `0`:
     - Neighbor `1` with weight `5`: Update `key[1] = 5`
     - Neighbor `2` with weight `1`: Update `key[2] = 1`
   - `key = [0, 5, 1]`

3. **Iteration 2:**
   - Find minimum key: `u = 2` (key[2] = 1)
   - Include vertex `2` in MST: `mst = [true, false, true]`
   - Add key[2] to `totalWeight`: `totalWeight = 1`
   - Update keys for neighbors of `2`:
     - Neighbor `0` with weight `1`: Already in MST, no update.
     - Neighbor `1` with weight `3`: Update `key[1] = 3`
   - `key = [0, 3, 1]`

4. **Iteration 3:**
   - Find minimum key: `u = 1` (key[1] = 3)
   - Include vertex `1` in MST: `mst = [true, true, true]`
   - Add key[1] to `totalWeight`: `totalWeight = 4`
   - Update keys for neighbors of `1`:
     - Neighbor `0` with weight `5`: Already in MST, no update.
     - Neighbor `2` with weight `3`: Already in MST, no update.
   - `key = [0, 3, 1]`

**Final MST Weight:**
- `totalWeight = 4`

### Conclusion

The dry run confirms that the algorithm correctly computes the MST and the total weight of the MST edges, which matches the expected output of `4` for the given example. The explanation, time complexity analysis, space complexity analysis, and dry run provide a comprehensive understanding of how the Prim's MST algorithm is implemented and executed in this code.


# Method 2 (Krushkal's Algorithm)
```cpp
```

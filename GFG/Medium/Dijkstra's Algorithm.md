# Method 1 (min heap)
### Code
```cpp
vector<int> dijkstra(int V, vector<vector<int>> adj[], int S) {
        vector<int> dist(V, INT_MAX);
        dist[S]=0;
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
        pq.push({0, S});
    
        while (!pq.empty()) {
            int d=pq.top().first;
            int node=pq.top().second;
            pq.pop();
            for (auto it:adj[node]) {
                int neighbor=it[0];
                int weight=it[1];
                if (dist[node]+weight<dist[neighbor]) {
                    dist[neighbor]=dist[node]+weight;
                    pq.push({dist[neighbor], neighbor});
                }
            }
        }
    
        return dist;
}
```
### Explanation

1. **Initialization**:
   - A vector `dist` of size `V` (number of vertices) is initialized with `INT_MAX`, representing infinite distance. This is done to ensure that all vertices are initially marked as unvisited with an infinite distance. The distance from the source vertex `S` to itself is set to `0` (`dist[S] = 0`).

2. **Priority Queue**:
   - A min-heap priority queue `pq` is used to efficiently retrieve the vertex with the smallest distance. The queue stores pairs `(distance, vertex)` to facilitate this process.

3. **Algorithm**:
   - The algorithm starts by inserting the source vertex into the priority queue with a distance of `0`. It then enters a loop that runs until the queue is empty.
   - In each iteration, the vertex with the smallest distance (`node`) is extracted from the queue. This is done using `pq.top()`, which returns the pair `(d, node)`, and then `pq.pop()` is called to remove it.
   - The algorithm iterates over all neighbors of the current vertex (`node`). For each neighbor, it calculates the potential new distance (`dist[node] + weight`) and checks if this new distance is smaller than the currently recorded distance (`dist[neighbor]`). If it is, the distance is updated, and the neighbor is pushed into the priority queue with the new distance.

4. **Output**:
   - The algorithm returns the `dist` vector, which contains the shortest distances from the source vertex `S` to all other vertices.

### Time Complexity Analysis

- The time complexity of Dijkstra's algorithm using a min-heap (priority queue) is \(O((V + E) \log V)\), where:
  - \(V\) is the number of vertices.
  - \(E\) is the number of edges.
- **Explanation**:
  - Each vertex is inserted into the priority queue at most once, which takes \(O(\log V)\) time per insertion.
  - Each edge is processed at most once, which involves an update operation in the priority queue, taking \(O(\log V)\) time.
  - Thus, the overall complexity is \(O((V + E) \log V)\).

### Space Complexity Analysis

- The space complexity is \(O(V + E)\), which includes:
  - \(O(V)\) for the `dist` vector to store the shortest distances to each vertex.
  - \(O(V)\) for the priority queue to store the vertices being processed.
  - \(O(E)\) for the adjacency list representation of the graph.

### Dry Run

Let's perform a dry run of the code on a simple example:

#### Example Graph:

- Number of vertices: \( V = 4 \)
- Edges: (0 → 1, weight = 1), (0 → 2, weight = 4), (1 → 2, weight = 2), (1 → 3, weight = 6), (2 → 3, weight = 3)
- Source vertex: \( S = 0 \)

#### Graph Representation (Adjacency List):

```
adj[0] = {{1, 1}, {2, 4}}
adj[1] = {{2, 2}, {3, 6}}
adj[2] = {{3, 3}}
adj[3] = {}
```

#### Initial State:

- `dist = [0, INF, INF, INF]` (where INF = INT_MAX)
- `pq = [(0, 0)]`

#### Iterations:

1. **First Iteration**:
   - `pq = [(0, 0)]`
   - Extract `(0, 0)`: `d = 0`, `node = 0`
   - Neighbors of `0`: `(1, 1)` and `(2, 4)`
   - Update `dist[1] = 1` and `dist[2] = 4`
   - `pq = [(1, 1), (4, 2)]`

2. **Second Iteration**:
   - `pq = [(1, 1), (4, 2)]`
   - Extract `(1, 1)`: `d = 1`, `node = 1`
   - Neighbors of `1`: `(2, 2)` and `(3, 6)`
   - Update `dist[2] = 3` (since `1 + 2 < 4`) and `dist[3] = 7` (since `1 + 6 < INF`)
   - `pq = [(3, 2), (4, 2), (7, 3)]`

3. **Third Iteration**:
   - `pq = [(3, 2), (4, 2), (7, 3)]`
   - Extract `(3, 2)`: `d = 3`, `node = 2`
   - Neighbors of `2`: `(3, 3)`
   - Update `dist[3] = 6` (since `3 + 3 < 7`)
   - `pq = [(4, 2), (7, 3), (6, 3)]`

4. **Fourth Iteration**:
   - `pq = [(4, 2), (6, 3), (7, 3)]`
   - Extract `(4, 2)`: `d = 4`, `node = 2`
   - No updates, as all neighbors have been processed.
   - `pq = [(6, 3), (7, 3)]`

5. **Fifth Iteration**:
   - `pq = [(6, 3), (7, 3)]`
   - Extract `(6, 3)`: `d = 6`, `node = 3`
   - No updates, as `node 3` has no neighbors.
   - `pq = [(7, 3)]`

6. **Sixth Iteration**:
   - `pq = [(7, 3)]`
   - Extract `(7, 3)`: `d = 7`, `node = 3`
   - No updates, as all neighbors have been processed.
   - `pq = []`
  
# Method 2 (set)
### C++ Implementation Using `set`

```cpp
#include <vector>
#include <set>
#include <limits.h>

using namespace std;

vector<int> dijkstra(int V, vector<vector<int>> adj[], int S) {
    // Initialize distances to all vertices as infinite, except for the source
    vector<int> dist(V, INT_MAX);
    dist[S] = 0;

    // Set to process vertices with the current shortest known distance
    set<pair<int, int>> st;
    
    // Insert source with distance 0
    st.insert({0, S});

    while (!st.empty()) {
        // Extract the vertex with the smallest distance
        auto it = st.begin();
        int d = it->first; // current distance
        int node = it->second; // current node
        st.erase(it);

        // Iterate over all the neighbors of the current node
        for (auto it : adj[node]) {
            int neighbor = it[0];
            int weight = it[1];

            // Check if a shorter path to the neighbor is found
            if (dist[node] + weight < dist[neighbor]) {
                // If the neighbor already has a distance value, remove it from the set
                if (dist[neighbor] != INT_MAX) {
                    st.erase({dist[neighbor], neighbor});
                }

                // Update the shortest distance and insert the new pair into the set
                dist[neighbor] = dist[node] + weight;
                st.insert({dist[neighbor], neighbor});
            }
        }
    }

    return dist;
}
```

### Key Differences and Explanation

1. **Initialization**:
   - Similar to the priority queue version, the `dist` vector is initialized with `INT_MAX`, and the distance to the source is set to `0`.

2. **Set Usage**:
   - A `set` named `st` is used to store pairs of `(distance, vertex)`. The `set` automatically keeps the elements sorted based on the first value of the pair (i.e., the distance). This allows us to efficiently retrieve and remove the vertex with the minimum distance.

3. **Handling Distance Updates**:
   - When a shorter path to a vertex is found, the existing distance and vertex pair are removed from the set (if present) and the new shorter distance is inserted. This step ensures that the `set` always contains the most up-to-date shortest paths.

4. **Edge Relaxation**:
   - The relaxation step is similar to the priority queue version. For each adjacent vertex, the algorithm checks if a shorter path can be found using the current vertex, updates the `dist` array, and inserts the updated distance into the set.

### Time Complexity Analysis

- The overall time complexity of this approach is still \(O((V + E) \log V)\). Each insertion, deletion, and update operation in the `set` takes \(O(\log V)\) time.
  - Each vertex can be inserted and removed from the set, which gives us \(O(V \log V)\).
  - Each edge is considered once for relaxation, and if it results in an update, it leads to an insertion and possibly deletion in the set, resulting in \(O(E \log V)\).

### Space Complexity Analysis

- The space complexity remains \(O(V + E)\):
  - \(O(V)\) for the `dist` vector.
  - \(O(V)\) for the `set` to store vertices.
  - \(O(E)\) for the adjacency list representation of the graph.

### Conclusion

Using a `set` is a viable alternative to using a `priority_queue` in Dijkstra's algorithm. It is particularly useful in scenarios where you need more control over the elements, such as ensuring unique vertex entries and updating existing entries. Both approaches are efficient, with similar time and space complexities, and choosing between them can depend on specific implementation needs and preferences.


#### Final Output:

- `dist = [0, 1, 3, 6]`

This result indicates the shortest distances from the source vertex `0` to all other vertices: `1`, `2`, and `3`.

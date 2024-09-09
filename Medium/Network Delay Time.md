### Code
```cpp
class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
    // Create an adjacency list to store the graph
    vector<vector<pair<int, int>>> graph(n+1);
    
    // Populate the graph
    for (const auto& time : times) {
        int u = time[0], v = time[1], w = time[2];
        graph[u].emplace_back(v, w);
    }
    
    // Use a priority queue to implement Dijkstra's algorithm (min-heap)
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
    
    // Distance vector to store the minimum time to reach each node, initialized to infinity
    vector<int> dist(n + 1, INT_MAX);
    
    // Start from the source node k, so the distance to k is 0
    dist[k] = 0;
    pq.push({0, k});  // {time, node}
    
    // Process the nodes
    while (!pq.empty()) {
        auto [time, node] = pq.top();
        pq.pop();
        
        // If we have already found a better path to this node, continue
        if (time > dist[node]) continue;
        
        // Explore the neighbors of the current node
        for (auto& [neighbor, weight] : graph[node]) {
            int newTime = time + weight;
            
            // If a shorter path is found, update and push to the queue
            if (newTime < dist[neighbor]) {
                dist[neighbor] = newTime;
                pq.push({newTime, neighbor});
            }
        }
    }
    
    // Find the maximum time among the reachable nodes
    int maxTime=0;
    for (int i=1; i<=n; ++i) {
        if (dist[i]==INT_MAX) return -1;
        maxTime=max(maxTime, dist[i]);
    }
    return maxTime;
}
};
```


### Explanation:

This problem can be solved using **Dijkstra's Algorithm**, which is designed to find the shortest paths from a source node to all other nodes in a graph with non-negative edge weights. The goal of this problem is to compute the minimum time it takes for a signal to reach all nodes in the network starting from a specific node `k`.

Here's a breakdown of the code:

1. **Graph Representation**:
   - The graph is represented as an **adjacency list**, where `graph[u]` contains a list of pairs `(v, w)` indicating that there is a directed edge from node `u` to node `v` with a weight (or time delay) `w`.

2. **Priority Queue (Min-Heap)**:
   - A **priority queue** is used to always explore the node with the shortest known distance (this is essential to Dijkstra's algorithm).
   - The priority queue stores pairs of the form `(time, node)`, where `time` is the current shortest time to reach `node`.

3. **Distance Array**:
   - The `dist` vector stores the minimum time required to reach each node. It is initialized to `INT_MAX` (infinity) for all nodes except the starting node `k`, which is initialized to `0`.

4. **Processing the Nodes**:
   - The algorithm processes nodes by continuously exploring the node with the shortest time in the priority queue.
   - For each node, its neighbors are checked, and if a shorter path to a neighbor is found, the time is updated, and the neighbor is added to the priority queue.

5. **Final Calculation**:
   - After all nodes are processed, the algorithm finds the maximum time in the `dist` array, which represents the time it took for the signal to reach the farthest node.
   - If any node is still unreachable (`dist[i] == INT_MAX`), the function returns `-1`. Otherwise, it returns the maximum time.

### Time Complexity:
- **Graph Construction**: Constructing the graph from the input takes \(O(E)\), where `E` is the number of edges.
- **Dijkstra’s Algorithm**: The algorithm runs in \(O((E + V) \log V)\), where `V` is the number of vertices (or nodes), and `E` is the number of edges. This is because each node is processed once, and each edge is relaxed at most once, while operations on the priority queue take \(O(\log V)\) time.
  
Thus, the total time complexity is:
- \(O((E + V) \log V)\), where:
  - `E` is the number of edges,
  - `V` is the number of nodes.

### Space Complexity:
- **Graph Storage**: The adjacency list takes \(O(E)\) space, where `E` is the number of edges.
- **Distance Array**: The distance array takes \(O(V)\) space, where `V` is the number of nodes.
- **Priority Queue**: The priority queue takes \(O(V)\) space in the worst case, where `V` is the number of nodes.

Thus, the total space complexity is:
- \(O(E + V)\), where `E` is the number of edges, and `V` is the number of nodes.

### Dry Run:

Let's dry run the example:

**Input**: `times = [[2,1,1],[2,3,1],[3,4,1]]`, `n = 4`, `k = 2`

1. **Graph Construction**:
   - The graph will look like:
     ```
     graph[2] = [(1, 1), (3, 1)]
     graph[3] = [(4, 1)]
     ```

2. **Initialization**:
   - `dist = [INT_MAX, INT_MAX, 0, INT_MAX, INT_MAX]` (distance to node `k = 2` is 0).
   - Priority queue `pq = [(0, 2)]` (start from node `k = 2` with time 0).

3. **Processing Nodes**:
   - **First Iteration**:
     - Pop `(0, 2)` from the queue.
     - Explore neighbors of node `2`: 
       - Neighbor `1`: new time = 0 + 1 = 1. Update `dist[1] = 1` and push `(1, 1)` into `pq`.
       - Neighbor `3`: new time = 0 + 1 = 1. Update `dist[3] = 1` and push `(1, 3)` into `pq`.
     - `pq = [(1, 1), (1, 3)]`, `dist = [INT_MAX, 1, 0, 1, INT_MAX]`.
   
   - **Second Iteration**:
     - Pop `(1, 1)` from the queue. No neighbors to explore.
     - `pq = [(1, 3)]`.
   
   - **Third Iteration**:
     - Pop `(1, 3)` from the queue.
     - Explore neighbors of node `3`: 
       - Neighbor `4`: new time = 1 + 1 = 2. Update `dist[4] = 2` and push `(2, 4)` into `pq`.
     - `pq = [(2, 4)]`, `dist = [INT_MAX, 1, 0, 1, 2]`.
   
   - **Fourth Iteration**:
     - Pop `(2, 4)` from the queue. No neighbors to explore.
     - `pq = []`.

4. **Final Calculation**:
   - Maximum time in `dist` array is 2.
   - All nodes are reachable, so return `2`.

**Output**: `2`.

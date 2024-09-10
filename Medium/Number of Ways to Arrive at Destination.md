### Code
```cpp
class Solution {
public:
    int countPaths(int n, vector<vector<int>>& roads) {
        const int MOD=1e9+7;
        vector<vector<pair<int, int>>> adj(n);
        for (auto& road:roads) {
            int u=road[0], v=road[1], time=road[2];
            adj[u].emplace_back(v, time);
            adj[v].emplace_back(u, time);
        }

        // min-heap with {time, node}
        priority_queue<pair<long long, int>, vector<pair<long long, int>>, greater<>> pq;
        vector<long long> dist(n, LLONG_MAX);
        vector<int> ways(n, 0);
        
        // Start Dijkstra from node 0
        dist[0]=0;
        ways[0]=1;
        pq.emplace(0, 0);
        
        while (!pq.empty()) {
            auto [time, u]=pq.top();
            pq.pop();
            if (time>dist[u]) continue;
            
            else{
                for (auto& [v, t]:adj[u]) {
                    long long new_time=time+t;
                    
                    // Found a shorter path to v
                    if (new_time<dist[v]) {
                        dist[v]=new_time;
                        pq.emplace(new_time, v);
                        ways[v]=ways[u];  // reset ways to reach v
                    }
                    // Found another shortest path to v
                    else if (new_time==dist[v]) {
                        ways[v]=(ways[v]+ways[u])%MOD;
                    }
                }
            }
        }
        return ways[n-1]; // Return the number of ways to reach node n-1
    }
};
```

### Explanation:

1. **Problem Overview:**
   You are given a city with `n` intersections and roads connecting them. Each road has a specific travel time. The task is to find how many distinct ways exist to travel from intersection `0` (the source) to intersection `n-1` (the destination) in the shortest amount of time.

2. **Dijkstra's Algorithm:**
   - Dijkstra’s algorithm is used to find the shortest path from the source node to all other nodes.
   - We will use Dijkstra's algorithm to compute the shortest path from intersection `0` to intersection `n-1`.
   - While computing the shortest path, we also count how many different ways can achieve this shortest path to each node.

3. **Key Steps:**
   - **Graph Representation:** We represent the graph as an adjacency list where each element is a pair `(neighbor, time)` representing a road between two intersections and the time it takes to travel.
   - **Priority Queue:** A priority queue is used to process nodes with the shortest time first.
   - **Distance Array:** This array keeps track of the shortest time to reach each node.
   - **Ways Array:** This array keeps track of how many ways we can reach each node in the shortest time.
   - For every road from a node `u` to a node `v`, check if traveling via `u` provides a shorter or equal travel time to reach `v`. If it's shorter, update the shortest time and reset the number of ways to reach `v`. If it's equal, just increment the number of ways.

4. **Modulo Operation:** Since the number of ways can be large, return the result modulo \(10^9 + 7\).

---

### Time Complexity:

- **Graph Construction:** Creating the adjacency list takes \(O(E)\), where \(E\) is the number of roads (edges).
- **Dijkstra's Algorithm:**
  - Each node is processed at most once, and each edge is relaxed at most once.
  - Using a priority queue, the time complexity is \(O((n + E) \log n)\), where \(n\) is the number of intersections (nodes), and \(E\) is the number of roads (edges). The \(\log n\) factor comes from priority queue operations (insertions and deletions).
  
Thus, the overall time complexity is **\(O((n + E) \log n)\)**.

---

### Space Complexity:

- **Adjacency List:** We need to store the graph as an adjacency list, which takes \(O(E)\) space, where \(E\) is the number of roads.
- **Distance and Ways Arrays:** Two arrays of size \(n\) (for the shortest distances and the number of ways to reach each node) take \(O(n)\) space.
- **Priority Queue:** In the worst case, the priority queue will store all \(n\) nodes, so it takes \(O(n)\) space.

Thus, the overall space complexity is **\(O(n + E)\)**.

---

### Dry Run (Example 1):

**Input:**

`n = 7`, `roads = [[0,6,7],[0,1,2],[1,2,3],[1,3,3],[6,3,3],[3,5,1],[6,5,1],[2,5,1],[0,4,5],[4,6,2]]`

**Step-by-Step:**

1. **Initialization:**
   - `dist[] = [0, ∞, ∞, ∞, ∞, ∞, ∞]` (0 is the starting point with distance 0)
   - `ways[] = [1, 0, 0, 0, 0, 0, 0]` (There is 1 way to reach node 0)
   - The priority queue starts with node 0: `pq = [(0, 0)]`

2. **Processing Node 0:**
   - Node 0 has edges to nodes 6, 1, and 4.
   - Relax these edges:
     - To node 6: New distance = 7, `dist[6] = 7`, `ways[6] = 1`, add `(7, 6)` to the queue.
     - To node 1: New distance = 2, `dist[1] = 2`, `ways[1] = 1`, add `(2, 1)` to the queue.
     - To node 4: New distance = 5, `dist[4] = 5`, `ways[4] = 1`, add `(5, 4)` to the queue.

3. **Processing Node 1:**
   - Node 1 has edges to nodes 0, 2, and 3.
   - Relax these edges:
     - To node 2: New distance = 5, `dist[2] = 5`, `ways[2] = 1`, add `(5, 2)` to the queue.
     - To node 3: New distance = 5, `dist[3] = 5`, `ways[3] = 1`, add `(5, 3)` to the queue.

4. **Processing Node 4:**
   - Node 4 has edges to node 0 and node 6.
   - To node 6: New distance = 7, which is equal to `dist[6]`, so we add another way to reach node 6: `ways[6] = 2`.

5. **Processing Node 2, 3, and Other Nodes:**
   - We continue processing nodes 2, 3, and 6.
   - For node 6: As we relax the edges from nodes 5 and 6, we find additional ways to reach node 6.
     - Final `dist[6] = 7`
     - Final `ways[6] = 4`

**Output:**
- There are **4** ways to reach node 6 in the shortest time, so the output is `4`.

---

### Final Output:

The solution efficiently finds that there are 4 distinct ways to travel from intersection `0` to intersection `6` in the shortest time, and the output for this example is `4`.

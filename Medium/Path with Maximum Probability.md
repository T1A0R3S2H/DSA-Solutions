### Code:
```cpp
class Solution {
public:
    double maxProbability(int n, vector<vector<int>>& edges, vector<double>& succProb, int start_node, int end_node) {

        // adj list banai
        vector<vector<pair<int, double>>> graph(n);
        for (int i=0; i<edges.size(); ++i) {
            int u=edges[i][0];
            int v=edges[i][1];
            double prob=succProb[i];
            graph[u].push_back({v, prob});
            graph[v].push_back({u, prob});
        }
        
        // max heap with (start with max possible probab yaani ki 1)
        priority_queue<pair<double, int>> pq;
        pq.push({1.0, start_node});
        
        // Vector to store the maximum probability to reach each node
        vector<double> maxProb(n, 0.0);
        maxProb[start_node]=1.0;
        
        while (!pq.empty()) {
            double curr=pq.top().first;
            int node=pq.top().second;
            pq.pop();
            
            if (node==end_node) {
                return curr;
            }

            // neighbours ke paas
            for (auto& neighbour:graph[node]) {
                int nbr=neighbour.first;
                double weight=neighbour.second;
                if (curr*weight>maxProb[nbr]) {
                    maxProb[nbr]=curr*weight;
                    pq.push({maxProb[nbr], nbr});
                }
            }
        }
        return 0.0;
    }
};
```

#### 1. **Explanation**:
The problem is about finding the path in an undirected weighted graph where each edge has a probability of success. The goal is to maximize the success probability from the start node to the end node. This is essentially a variation of the shortest path problem but instead of summing the edge weights, we multiply probabilities.

We adapt **Dijkstra's algorithm** to solve this, as it efficiently finds the shortest path in weighted graphs. Here, we modify it to track and maximize the product of probabilities instead of minimizing distances.

#### 2. **Time Complexity**:
- **Building the graph**: The time complexity for building the adjacency list is `O(E)`, where `E` is the number of edges.
- **Priority queue operations**: The priority queue will perform operations proportional to `O((E + V) * log V)` because we may process each edge once, and the priority queue is updated at most `V` times.
  
Overall, the time complexity is **O(E * log V)** where `E` is the number of edges and `V` is the number of vertices (nodes).

#### 3. **Space Complexity**:
- **Graph storage**: We store the graph as an adjacency list, which takes `O(E)` space.
- **Priority queue**: The priority queue stores up to `V` elements, leading to `O(V)` space complexity.
- **Other data**: We use an array `maxProb` to store the maximum probabilities, which is of size `V`.
  
Thus, the space complexity is **O(V + E)**.

#### 4. **Dry Run**:

Let’s take the input:
```cpp
n = 3, 
edges = [[0,1],[1,2],[0,2]], 
succProb = [0.5, 0.5, 0.2], 
start = 0, 
end = 2
```

- **Step 1**: Build the adjacency list.
  ```
  graph[0] = [(1, 0.5), (2, 0.2)]
  graph[1] = [(0, 0.5), (2, 0.5)]
  graph[2] = [(0, 0.2), (1, 0.5)]
  ```

- **Step 2**: Initialize the priority queue with the start node (node `0`) and probability `1.0`:
  ```
  pq = [(1.0, 0)] 
  maxProb = [1.0, 0.0, 0.0]
  ```

- **Step 3**: Pop node `0` from the priority queue. The current probability is `1.0`. Check its neighbors:
  - Neighbor `1`: Probability = `1.0 * 0.5 = 0.5`. Update `maxProb[1] = 0.5` and push `(0.5, 1)` to the queue.
  - Neighbor `2`: Probability = `1.0 * 0.2 = 0.2`. Update `maxProb[2] = 0.2` and push `(0.2, 2)` to the queue.
  
  Now:
  ```
  pq = [(0.5, 1), (0.2, 2)]
  maxProb = [1.0, 0.5, 0.2]
  ```

- **Step 4**: Pop node `1` from the priority queue. The current probability is `0.5`. Check its neighbors:
  - Neighbor `0`: Already has a higher probability (`1.0`), so no update.
  - Neighbor `2`: Probability = `0.5 * 0.5 = 0.25`. Update `maxProb[2] = 0.25` and push `(0.25, 2)` to the queue.
  
  Now:
  ```
  pq = [(0.25, 2), (0.2, 2)]
  maxProb = [1.0, 0.5, 0.25]
  ```

- **Step 5**: Pop node `2` from the priority queue. The current probability is `0.25`, and it's the target node (`end = 2`), so we return `0.25`.

### Conclusion:
- The maximum probability from node `0` to node `2` is `0.25`.

Let me know if you need more details!

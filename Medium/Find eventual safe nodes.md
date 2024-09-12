### Code:
```cpp
class Solution {
public:
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        int n=graph.size();
        vector<int> outdegree(n, 0);
        vector<vector<int>> reverseGraph(n);
        queue<int> q;
        vector<int> safeNodes;

        // terminal nodes and build reverse graph
        for (int i=0; i<n; ++i) {
            if (graph[i].empty()) {
                q.push(i); // Terminal node
            } 
            else {
                for (int node:graph[i]) {
                    reverseGraph[node].push_back(i); // Build reverse graph
                }
            }
            outdegree[i]=graph[i].size(); // Track outgoing edges count
        }

        // process terminal nodes and reduce outdegree of incoming nodes
        while (!q.empty()) {
            int current=q.front();
            q.pop();
            safeNodes.push_back(current);
            // Check incoming nodes in reverse graph
            for (int in:reverseGraph[current]) {
                outdegree[in]--;
                if (outdegree[in]==0) {
                    q.push(in); // no more outgoing, so safe
                }
            }
        }
        sort(safeNodes.begin(), safeNodes.end());
        return safeNodes;
    }
};
```
#### Explanation:
1. **Terminal Nodes Identification**: 
   - First, we identify all terminal nodes (those with no outgoing edges). These nodes are automatically safe since there is no outgoing edge to any other node. We push these nodes into a queue.
   
2. **Reverse Graph Construction**:
   - We build a reverse graph, where for each edge from node `u` to node `v` in the original graph, we add an edge from `v` to `u` in the reverse graph. This reverse graph helps in finding which nodes point to the terminal nodes.

3. **Outdegree Count**:
   - For each node, we count the number of outgoing edges. As we process terminal nodes, we reduce the outdegree of the nodes pointing to these terminal nodes.

4. **Processing the Queue**:
   - We process terminal nodes first, and for each terminal node, we reduce the outdegree of the incoming nodes in the reverse graph. When a node’s outdegree becomes zero, it means it can only point to safe nodes, so we add it to the queue.

5. **Sorting Safe Nodes**:
   - Once all safe nodes are identified, we sort the result in ascending order and return the list.

---

#### Time Complexity:
- **O(V + E)** where `V` is the number of nodes and `E` is the number of edges.
  - We traverse each node and each edge exactly once while constructing the reverse graph and processing the nodes.

#### Space Complexity:
- **O(V + E)**:
  - We use `O(V)` space for the outdegree array and `O(E)` for storing the reverse graph.

---

#### Dry Run:

Consider the input:

`graph = [[1,2],[2,3],[5],[0],[5],[],[]]`

1. **Step 1: Terminal Nodes**:
   - Node `5` and `6` are terminal nodes (no outgoing edges).
   - We push `5` and `6` into the queue.

2. **Step 2: Reverse Graph**:
   - We build the reverse graph:
     - `1 -> 0` and `2 -> 0`
     - `2 -> 1` and `3 -> 1`
     - `5 -> 2` and `5 -> 4`
     - `0 -> 3`

3. **Step 3: Outdegree Array**:
   - Initialize the outdegree array:
     - `outdegree = [2, 2, 1, 1, 1, 0, 0]`
   - Node `5` and `6` have outdegree `0` (terminal nodes).

4. **Step 4: Process Queue**:
   - Start processing node `5`:
     - For node `2`, reduce outdegree from `1` to `0`, so push `2` into the queue.
     - For node `4`, reduce outdegree from `1` to `0`, so push `4` into the queue.
   - Process node `6` (no incoming edges).
   - Process node `2`:
     - For node `0`, reduce outdegree from `2` to `1`.
     - For node `1`, reduce outdegree from `2` to `1`.
   - Process node `4`.
   - Process node `0` and node `1`, but their outdegrees don’t become zero.

5. **Step 5: Result**:
   - Collect the safe nodes: `[2, 4, 5, 6]`.
   - Sort them to get `[2, 4, 5, 6]`.

---

#### Final Result:
The safe nodes are `[2, 4, 5, 6]`.


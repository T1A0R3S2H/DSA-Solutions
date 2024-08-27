### Code
```cpp
class Solution {
  public:
    // Function to return the adjacency list for each vertex.
    vector<vector<int>> printGraph(int V, vector<pair<int,int>>edges) {
        vector<vector<int>> adj(V);
        for(auto i=0;i<edges.size();i++){
            int u=edges[i].first;
            int v=edges[i].second;
            adj[u].push_back(v);
            adj[v].push_back(u);
        }
        return adj;
    }
};
```
### Explanation

The problem requires constructing and returning an adjacency list representation of an undirected graph, given the number of vertices `V` and a list of edges. Each edge connects two vertices in an undirected manner, meaning if there's an edge between `u` and `v`, then `v` is a neighbor of `u` and vice versa.

**Code Breakdown:**
1. **Initialization of the Adjacency List:** 
   - `vector<vector<int>> adj(V);` creates a vector of vectors called `adj` to store the adjacency list. The outer vector has a size of `V`, corresponding to the number of vertices, and each inner vector will hold the adjacent vertices for each vertex.
   
2. **Processing the Edges:**
   - The code iterates over the `edges` vector, which contains pairs representing edges. For each edge `(u, v)`, the code does the following:
     - Adds `v` to the adjacency list of `u`.
     - Adds `u` to the adjacency list of `v` (since the graph is undirected).
   
3. **Return Statement:**
   - The adjacency list `adj` is returned, representing the graph's structure.

### Time Complexity Analysis

- **Loop Over Edges:** The loop runs for each edge in the `edges` vector, so the time complexity of this loop is \(O(E)\), where \(E\) is the number of edges.
- **Inside the Loop:** Each operation inside the loop (push back operations) runs in \(O(1)\).
  
Overall, the time complexity is **\(O(E)\)**.

### Space Complexity Analysis

- **Adjacency List Storage:** The adjacency list `adj` requires space proportional to the number of vertices \(V\) and the number of edges \(E\), since each edge adds two entries (one for each vertex) in the list.
  
Therefore, the space complexity is **\(O(V + E)\)**.

### Dry Run

Let's perform a dry run with a simple example.

- **Input:**
  - `V = 3` (3 vertices: 0, 1, 2)
  - `edges = [(0, 1), (0, 2), (1, 2)]`

- **Execution:**
  1. Initialize `adj` as `[[ ], [ ], [ ]]`.
  2. Process edge `(0, 1)`:
     - Add `1` to the list of `0`: `adj[0].push_back(1)` -> `adj` becomes `[[1], [ ], [ ]]`.
     - Add `0` to the list of `1`: `adj[1].push_back(0)` -> `adj` becomes `[[1], [0], [ ]]`.
  3. Process edge `(0, 2)`:
     - Add `2` to the list of `0`: `adj[0].push_back(2)` -> `adj` becomes `[[1, 2], [0], [ ]]`.
     - Add `0` to the list of `2`: `adj[2].push_back(0)` -> `adj` becomes `[[1, 2], [0], [0]]`.
  4. Process edge `(1, 2)`:
     - Add `2` to the list of `1`: `adj[1].push_back(2)` -> `adj` becomes `[[1, 2], [0, 2], [0]]`.
     - Add `1` to the list of `2`: `adj[2].push_back(1)` -> `adj` becomes `[[1, 2], [0, 2], [0, 1]]`.

- **Output:**
  - The final adjacency list representation: `[[1, 2], [0, 2], [0, 1]]`.

This output matches the expected adjacency list for the given input, demonstrating that the code correctly builds the adjacency list representation of an undirected graph.

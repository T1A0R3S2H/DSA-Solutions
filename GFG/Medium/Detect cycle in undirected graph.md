# DFS
### Code
```cpp
class Solution {
  public:
    // Function to detect cycle in an undirected graph.
    bool isCycle(int V, vector<int> adj[]) {
        vector<int> visited(V, 0); // To track visited nodes
        
        // Iterate through all nodes to handle disconnected graphs
        for (int i=0; i<V; i++) {
            if (!visited[i]) {
                // If a cycle is found, return true
                if (dfs(i, -1, visited, adj)) {
                    return true;
                }
            }
        }
        
        return false;
    }
    
  private:
    bool dfs(int node, int parent, vector<int> &visited, vector<int> adj[]) {
        visited[node] = 1; // Mark the current node as visited
        
        // Traverse all adjacent nodes
        for (auto &it : adj[node]) {
            if (!visited[it]) {
                // Recur for the adjacent node
                if (dfs(it, node, visited, adj)) {
                    return true;
                }
            }
            // If an adjacent node is visited and is not the parent, a cycle is found
            else if (it != parent) {
                return true;
            }
        }
        
        return false;
    }
};
```
### Explanation

1. **Initialization**: 
   - The `isCycle` function initializes a `visited` vector of size `V` with all values set to `0`, indicating that no nodes have been visited yet.
   - A loop iterates through all nodes (from `0` to `V-1`). This is necessary to handle disconnected graphs where not all nodes are reachable from a single starting point.

2. **DFS Function**: 
   - The private `dfs` function is defined to perform a depth-first search. It takes four parameters: `node` (the current node), `parent` (the parent of the current node to avoid trivial cycles), `visited` (the array to track visited nodes), and `adj` (the adjacency list of the graph).
   - The current node is marked as visited by setting `visited[node] = 1`.
   - For each adjacent node (`it`) of the current node:
     - If the adjacent node (`it`) has not been visited, the function calls itself recursively with `it` as the new node and `node` as its parent.
     - If the adjacent node (`it`) is already visited and is not the parent of the current node, a cycle is detected, and the function returns `true`.
   - If none of the adjacent nodes cause the function to return `true`, it means no cycle was found, and the function returns `false`.

3. **Cycle Detection**:
   - If any call to `dfs` returns `true`, indicating a cycle, the `isCycle` function returns `true`.
   - If no cycles are found after checking all nodes, the function returns `false`.

### Time Complexity Analysis

- The time complexity of this algorithm is **O(V + E)**, where `V` is the number of vertices and `E` is the number of edges in the graph.
  - Each vertex is visited once.
  - Each edge is traversed once when exploring adjacent nodes.

### Space Complexity Analysis

- The space complexity is **O(V)**:
  - The `visited` vector takes O(V) space.
  - The recursion stack can go as deep as the number of vertices in the worst case, leading to O(V) space.

### Dry Run

Let's dry run the code on a sample undirected graph to see how it works. Consider the following graph:

```
   0
  / \
 1   2
     | \
     |  3
     | /
     4
```

- **Vertices (V)**: 5 (numbered 0 to 4)
- **Edges (E)**: 5 (0-1, 0-2, 2-3, 3-4, 4-2)

- **Adjacency List Representation**:
  ```
  0 -> [1, 2]
  1 -> [0]
  2 -> [0, 3]
  3 -> [2, 4]
  4 -> [3, 2]
  ```

#### Steps in the Dry Run

1. **Initialization**:
   - `visited = [0, 0, 0, 0, 0]`

2. **First Loop Iteration (i = 0)**:
   - `visited[0] = 0`, so we call `dfs(0, -1, visited, adj)`.

3. **DFS(0, -1)**:
   - Mark `0` as visited: `visited = [1, 0, 0, 0, 0]`.
   - Check adjacency list of `0`:
     - `it = 1`:
       - `visited[1] = 0`, so we call `dfs(1, 0, visited, adj)`.
   
4. **DFS(1, 0)**:
   - Mark `1` as visited: `visited = [1, 1, 0, 0, 0]`.
   - Check adjacency list of `1`:
     - `it = 0`:
       - `visited[0] = 1` and `it` is the parent of `1`, so no cycle detected, continue.
   - Return `false` from `dfs(1, 0)`.

5. **Back to DFS(0, -1)**:
   - Check next adjacent node:
     - `it = 2`:
       - `visited[2] = 0`, so we call `dfs(2, 0, visited, adj)`.

6. **DFS(2, 0)**:
   - Mark `2` as visited: `visited = [1, 1, 1, 0, 0]`.
   - Check adjacency list of `2`:
     - `it = 0`:
       - `visited[0] = 1` and `it` is the parent of `2`, so no cycle detected, continue.
     - `it = 3`:
       - `visited[3] = 0`, so we call `dfs(3, 2, visited, adj)`.

7. **DFS(3, 2)**:
   - Mark `3` as visited: `visited = [1, 1, 1, 1, 0]`.
   - Check adjacency list of `3`:
     - `it = 2`:
       - `visited[2] = 1` and `it` is the parent of `3`, so no cycle detected, continue.
     - `it = 4`:
       - `visited[4] = 0`, so we call `dfs(4, 3, visited, adj)`.

8. **DFS(4, 3)**:
   - Mark `4` as visited: `visited = [1, 1, 1, 1, 1]`.
   - Check adjacency list of `4`:
     - `it = 3`:
       - `visited[3] = 1` and `it` is the parent of `4`, so no cycle detected, continue.
     - `it = 2`:
       - `visited[2] = 1` and `it` is not the parent of `4`, so a cycle is detected.
   - Return `true` from `dfs(4, 3)`.

9. **Backtrack**: Return `true` from all the previous `dfs` calls, eventually returning `true` from `isCycle`.

### Conclusion

The function successfully detects a cycle in the graph by using DFS, indicating that the cycle detection logic is correct and efficient. The cycle in the graph is detected at node `4` connecting back to node `2` through node `3`, confirming the presence of a cycle.





---
# BFS
### Code
```cpp
class Solution {
  public:
    // Function to detect cycle in an undirected graph.
    bool isCycle(int V, vector<int> adj[]) {
        queue<int> q;
        vector<int> parent(V);
        vector<int> visited(V, 0);

        // finding the source
        for(int i=0;i<V;i++){
            if(!visited[i]){
                q.push(i);

                while(!q.empty()){
                    int node=q.front();
                    q.pop();
                    visited[node]=true;
                    for(auto &it:adj[node]){
                        if(!visited[it]){
                            parent[it]=node;
                            q.push(it);
                        }
                        else if(visited[it] && parent[node]!=it){
                            return true;
                        }
                    }
                }
            }
        }
        return false;
    }
};
```

### Explanation

The code implements a cycle detection algorithm for an undirected graph using Breadth-First Search (BFS). The algorithm utilizes a queue to perform BFS and keeps track of visited nodes and their parent nodes. The main idea is to detect back edges during traversal, which would indicate the presence of a cycle in the graph.

**Key components of the code:**

1. **Queue (`queue<int> q`)**: This is used for BFS traversal. Nodes are added to the queue when they are first discovered, and they are processed in a first-in, first-out (FIFO) order.

2. **Parent Vector (`vector<int> parent`)**: This vector keeps track of the parent of each node during BFS traversal. This is crucial because, in an undirected graph, traversing back to the parent node does not indicate a cycle. The parent vector helps distinguish between revisiting a parent (which is normal) and detecting a cycle (revisiting a non-parent node).

3. **Visited Vector (`vector<int> visited`)**: This vector tracks which nodes have been visited. It's initialized with zeros, meaning no nodes are visited initially. When a node is visited, it's marked as `1`.

**Steps in the algorithm:**

1. **Initialization**: Initialize the `visited` and `parent` vectors and declare an empty queue for BFS.

2. **Loop through all nodes**: The `for` loop iterates over all nodes (`i = 0` to `V-1`). This is to handle disconnected components. If a node has not been visited, it's treated as a new source node for BFS.

3. **Start BFS**: If the current node `i` is unvisited, it is pushed into the queue to start BFS. The BFS will explore all reachable nodes from this starting node.

4. **BFS Traversal**:
    - While the queue is not empty, dequeue a node and mark it as visited.
    - For each adjacent node (`it`) of the current node:
      - If the adjacent node is not visited, mark it as visited, set its parent as the current node, and push it into the queue.
      - If the adjacent node is visited and is not the parent of the current node, a cycle is detected.

5. **Return result**: If no cycle is detected after processing all nodes, the function returns `false`. If a cycle is found during BFS, it returns `true`.

### Time Complexity

- **Initialization**: Initializing the `visited` and `parent` vectors takes O(V) time, where V is the number of vertices.
- **BFS Traversal**: Each vertex is enqueued and dequeued once, and each edge is checked once. Thus, the traversal complexity is O(V + E), where E is the number of edges.

Overall, the time complexity is **O(V + E)**.

### Space Complexity

- **Visited and Parent Vectors**: Each requires O(V) space for storing V elements.
- **Queue**: In the worst case, the queue can hold all vertices, requiring O(V) space.

Overall, the space complexity is **O(V)**.

### Dry Run

Consider a graph represented as:

```
  1 -- 0
  |    |
  2 -- 3
```

Adjacency list representation:

- `adj[0] = {1, 3}`
- `adj[1] = {0, 2}`
- `adj[2] = {1, 3}`
- `adj[3] = {0, 2}`

**Step-by-step execution:**

1. **Initialization**:
   - `visited = [0, 0, 0, 0]`
   - `parent = [0, 0, 0, 0]`
   - `q = empty`

2. **Loop through vertices**: Start with vertex `0` because it is not visited.

3. **Visit vertex 0**:
   - `q = [0]`
   - `visited = [1, 0, 0, 0]`
   - Dequeue `0`, explore its neighbors `1` and `3`.
   - `q = [1, 3]`
   - `visited = [1, 1, 0, 1]`
   - `parent = [0, 0, 0, 0]` (implicitly, parent of 1 is set to 0 and parent of 3 is set to 0 in the code flow)

4. **Visit vertex 1**:
   - `q = [3]`
   - Dequeue `1`, explore its neighbor `2`.
   - `q = [3, 2]`
   - `visited = [1, 1, 1, 1]`
   - `parent = [0, 0, 1, 0]` (parent of 2 is set to 1)

5. **Visit vertex 3**:
   - `q = [2]`
   - Dequeue `3`, explore its neighbors `0` and `2`.
   - `2` is visited and is not the parent of `3` (parent of 3 is `0`, but `2` is found as a neighbor). 
   - A cycle is detected!

The function returns `true` because a cycle is found.

### Conclusion

The algorithm effectively detects cycles in an undirected graph using BFS and parent tracking. The time complexity of O(V + E) makes it efficient for large graphs. The dry run illustrates how the algorithm tracks visited nodes and uses the parent to detect cycles by checking for back edges. This approach handles both connected and disconnected graphs, ensuring that all nodes and components are checked for cycles.

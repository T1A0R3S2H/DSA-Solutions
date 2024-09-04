# Method 1 (BFS)
### Code
```cpp
class Solution {
public:
    bool check(int start, int V, vector<vector<int>>& graph, int color[]) {
        queue<int> q;
	    q.push(start); 
	    color[start] = 0; 
	    while(!q.empty()) {
	        int node=q.front();
	        q.pop(); 
	        
	        for(auto it:graph[node]) {
	            // if the adjacent node is yet not colored
	            // you will give the opposite color of the node 
	            if(color[it]==-1) {
	                
	                color[it]=!color[node]; 
	                q.push(it); 
	            }
	            // is the adjacent guy having the same color 
	            // someone did color it on some other path 
	            else if(color[it]==color[node]) {
	                return false; 
	            }
	        }
	    }
	    return true; 
    }


    bool isBipartite(vector<vector<int>>& graph) {
        int V=graph.size();
	    int color[V]; 
	    for(int i=0; i<V; i++) color[i]=-1; 
	    
	    for(int i = 0;i<V;i++) {
	        // if not coloured
	        if(color[i]==-1) {
	            if(check(i, V, graph, color)==false) {
	                return false; 
	            }
	        }
	    }
	    return true;
    }
};
```
### Explanation:

A graph is bipartite if we can divide its set of vertices into two independent subsets such that every edge connects a vertex in one subset to a vertex in the other. This can be checked using BFS by trying to color the graph with two colors (0 and 1).

**Steps:**
1. **Initialization**: We initialize a `color` array to store the color assigned to each vertex. Initially, all vertices are uncolored (`-1`).

2. **BFS Traversal**:
   - For each uncolored vertex, we start a BFS. The starting vertex is colored with `0`.
   - We process each vertex in the queue:
     - For each adjacent vertex, if it is uncolored, we color it with the opposite color of the current vertex.
     - If an adjacent vertex has the same color as the current vertex, the graph is not bipartite, and we return `false`.

3. **Check All Components**: Since the graph might be disconnected, we repeat the process for every uncolored vertex. If all components pass the bipartite check, we return `true`.

### Time Complexity:

- **BFS Complexity**: Each vertex is added to the queue exactly once, and each edge is checked exactly once.
- **Overall Complexity**: The time complexity is `O(V + E)`, where `V` is the number of vertices and `E` is the number of edges in the graph.

### Space Complexity:

- **Color Array**: The `color` array requires `O(V)` space to store the color of each vertex.
- **Queue**: The queue used for BFS also requires `O(V)` space in the worst case.
- **Overall Complexity**: The space complexity is `O(V)`.

### Dry Run:

Let's perform a dry run on a simple example graph:

```plaintext
0 -- 1
|    |
2 -- 3
```

**Graph Representation**:
```plaintext
graph = [
  [1, 2],  // Edges from vertex 0 to 1, 2
  [0, 3],  // Edges from vertex 1 to 0, 3
  [0, 3],  // Edges from vertex 2 to 0, 3
  [1, 2]   // Edges from vertex 3 to 1, 2
]
```

**Step-by-Step Dry Run**:

1. **Initialization**:
   - `V = 4`
   - `color = [-1, -1, -1, -1]`

2. **Starting BFS from Vertex 0**:
   - Color vertex 0 with `0`.
   - `color = [0, -1, -1, -1]`
   - Add vertex 0 to the queue.

3. **Processing Vertex 0**:
   - Dequeue vertex 0.
   - Check its adjacent vertices:
     - Vertex 1 is uncolored. Color it with `1` and add to the queue.
     - Vertex 2 is uncolored. Color it with `1` and add to the queue.
   - `color = [0, 1, 1, -1]`

4. **Processing Vertex 1**:
   - Dequeue vertex 1.
   - Check its adjacent vertices:
     - Vertex 0 is already colored with `0` (different from vertex 1's color `1`).
     - Vertex 3 is uncolored. Color it with `0` and add to the queue.
   - `color = [0, 1, 1, 0]`

5. **Processing Vertex 2**:
   - Dequeue vertex 2.
   - Check its adjacent vertices:
     - Vertex 0 is already colored with `0` (different from vertex 2's color `1`).
     - Vertex 3 is already colored with `0` (different from vertex 2's color `1`).

6. **Processing Vertex 3**:
   - Dequeue vertex 3.
   - Check its adjacent vertices:
     - Vertex 1 is already colored with `1` (different from vertex 3's color `0`).
     - Vertex 2 is already colored with `1` (different from vertex 3's color `0`).

7. **Finish BFS**: The queue is empty, and no conflicts were found. The graph is bipartite.

8. **Check Remaining Vertices**:
   - All vertices are already colored, so we skip further BFS checks.

9. **Conclusion**: The graph is bipartite, and the function returns `true`.
---

# Method 2 (DFS)
### Code
```cpp
class Solution {
private: 
    bool dfs(int node, int col, int color[], vector<vector<int>>& graph) {
        color[node]=col; 
        
        // traverse adjacent nodes
        for(auto it:graph[node]) {
            // if uncoloured
            if(color[it]==-1) {
                if(dfs(it, !col, color, graph)==false) return false; 
            }
            // if previously coloured and have the same colour
            else if(color[it]==col) {
                return false; 
            }
        }
        return true; 
    }

public:
	bool isBipartite(vector<vector<int>>& graph){
        int V=graph.size();
	    int color[V];
	    for(int i=0; i<V; i++) color[i]=-1; 
	    
	    // for connected components
	    for(int i=0; i<V; i++) {
	        if(color[i]==-1) {
	            if(dfs(i, 0, color, graph)==false) 
	                return false; 
	        }
	    }
	    return true; 
	}
};
```

### Explanation

The problem at hand is to determine whether a given graph is bipartite. A graph is bipartite if it is possible to color its vertices using two colors in such a way that no two adjacent vertices share the same color.

This solution uses Depth-First Search (DFS) to attempt to color the graph. The idea is to start with any uncolored node, color it with one color (say 0), and then recursively try to color its adjacent nodes with the opposite color (1). If at any point you find an adjacent node that is already colored with the same color, the graph is not bipartite.

### Code Breakdown

1. **DFS Function (`dfs`)**: 
   - **Parameters**: 
     - `node`: The current node being visited.
     - `col`: The color to assign to the current node.
     - `color[]`: An array that stores the color of each node.
     - `graph`: The adjacency list representing the graph.
   - **Logic**:
     - Color the current node with `col`.
     - Traverse its adjacent nodes. 
     - If an adjacent node is uncolored, recursively call `dfs` with the opposite color.
     - If an adjacent node is already colored with the same color, return `false`, indicating the graph cannot be bipartite.
     - If all adjacent nodes can be properly colored, return `true`.

2. **isBipartite Function**:
   - **Parameters**:
     - `graph`: The adjacency list of the graph.
   - **Logic**:
     - Initialize the `color[]` array with `-1` for all nodes, indicating they are uncolored.
     - Iterate through all nodes in the graph to ensure all connected components are checked.
     - If any node is uncolored, start a DFS from that node. If DFS returns `false`, return `false` immediately.
     - If all nodes are processed without conflict, return `true`.

### Time Complexity

- **Time Complexity**: \(O(V + E)\)
  - Where \(V\) is the number of vertices and \(E\) is the number of edges.
  - The DFS traversal visits each node once and each edge once.

- **Space Complexity**: \(O(V)\)
  - Space is required for the `color[]` array and the recursion stack in DFS, which in the worst case (a linear graph) can go up to \(O(V)\).

### Dry Run

Let's dry run the algorithm on a simple graph:

#### Example Graph:

```
0 -- 1
|    |
3 -- 2
```

- The adjacency list:
  ```
  graph = {
      {1, 3}, // 0
      {0, 2}, // 1
      {1, 3}, // 2
      {0, 2}  // 3
  }
  ```

#### Dry Run Steps:

1. **Initial Setup**:
   - `color = [-1, -1, -1, -1]`
   - Start with node `0` (uncolored).

2. **DFS from node 0**:
   - Color `0` with `0`.
   - Move to adjacent node `1`.
   - Color `1` with `1`.

3. **DFS from node 1**:
   - Move to adjacent node `0`, already colored `0` (different from `1`), continue.
   - Move to adjacent node `2`.
   - Color `2` with `0`.

4. **DFS from node 2**:
   - Move to adjacent node `1`, already colored `1` (different from `0`), continue.
   - Move to adjacent node `3`.
   - Color `3` with `1`.

5. **DFS from node 3**:
   - Move to adjacent node `0`, already colored `0` (different from `1`), continue.
   - Move to adjacent node `2`, already colored `0` (different from `1`), continue.

6. **Completion**:
   - All nodes are colored without conflicts.
   - The graph is bipartite, return `true`.

This dry run shows how the algorithm ensures that the graph can be split into two sets with no internal edges, confirming its bipartiteness.

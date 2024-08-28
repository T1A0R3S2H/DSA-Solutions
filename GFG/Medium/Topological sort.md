# Method 1 (DFS)
### Code
```cpp
class Solution {
	public:
	void dfs(int start, vector<bool> &visited, stack<int> &s, vector<int> adj[]) {
	    visited[start]=1;
	    for(auto& it:adj[start]) {
	        if(!visited[it]) {
	            dfs(it, visited, s, adj);
	        }
	    }
	    s.push(start);
	}
	
	vector<int> topoSort(int V, vector<int> adj[]) {
	    vector<bool> visited(V);
	    stack<int> s;
	    for(int i=0; i<V; i++) {
	        if(!visited[i]) {
	            dfs(i, visited, s, adj);
	        }
	    }
	    vector<int> res;
	    for(int i=0; i<V; i++) {
	        res.push_back(s.top()); 
	        s.pop();
	    }
	    return res;
	}
};
```
### Explanation of the Code

The code implements a topological sorting algorithm for a Directed Acyclic Graph (DAG) using Depth-First Search (DFS). The primary idea is to use DFS to explore each vertex of the graph and use a stack to maintain the topological order.

1. **DFS Function**: 
   - The `dfs()` function is a helper function that takes a starting node `start`, a `visited` vector, a stack `s`, and an adjacency list `adj[]` as input.
   - It marks the current node as visited.
   - It iterates over all the adjacent vertices of the current node. If an adjacent vertex has not been visited, it recursively calls `dfs()` on that vertex.
   - After exploring all adjacent vertices, it pushes the current node onto the stack.

2. **Topological Sort Function (`topoSort`)**:
   - Initializes a `visited` vector to keep track of visited vertices.
   - Initializes a stack `s` to store the topological order of vertices.
   - Iterates through all vertices, and if a vertex has not been visited, it calls the `dfs()` function starting from that vertex.
   - Once all vertices have been processed, it pops elements from the stack and stores them in the result vector `res`.
   - Returns the result vector, which contains the topological order of the vertices.

### Time Complexity

- **Time Complexity**: O(V + E)
  - The DFS function runs for each vertex once, taking O(V) time.
  - For each vertex, it explores all its adjacent vertices, contributing to O(E) time.
  - Thus, the overall complexity is O(V + E).

### Space Complexity

- **Space Complexity**: O(V)
  - The `visited` vector takes O(V) space.
  - The stack can contain up to V elements in the worst case.
  - The result vector `res` also takes O(V) space.
  - The auxiliary stack space used in the recursive DFS calls could also be O(V) in the worst case.

### Dry Run

Consider a simple graph:

```
  0 → 1 → 2
  |
  ↓
  3 → 4
```

- **Vertices**: 5 (0 to 4)
- **Edges**: 5 (0→1, 0→3, 1→2, 3→4)

1. **Initial State**:
   - `visited`: [0, 0, 0, 0, 0]
   - `stack`: empty
   - `res`: empty

2. **DFS from Vertex 0**:
   - Mark 0 as visited: `visited`: [1, 0, 0, 0, 0]
   - Explore 1: `visited`: [1, 1, 0, 0, 0]
     - Explore 2: `visited`: [1, 1, 1, 0, 0]
     - Push 2 onto the stack: `stack`: [2]
   - Push 1 onto the stack: `stack`: [2, 1]
   - Explore 3: `visited`: [1, 1, 1, 1, 0]
     - Explore 4: `visited`: [1, 1, 1, 1, 1]
     - Push 4 onto the stack: `stack`: [2, 1, 4]
   - Push 3 onto the stack: `stack`: [2, 1, 4, 3]
   - Push 0 onto the stack: `stack`: [2, 1, 4, 3, 0]

3. **Processing Remaining Vertices**:
   - Vertices 1, 2, 3, and 4 are already visited, so no further DFS calls are made.

4. **Generating Result**:
   - Pop from the stack to get the topological order: `res`: [0, 3, 4, 1, 2]

The topological order of the given graph is `[0, 3, 4, 1, 2]`. Note that other valid topological orders are also possible.

This algorithm ensures that for any directed edge u -> v, vertex u appears before vertex v in the topological order, as required for a DAG.

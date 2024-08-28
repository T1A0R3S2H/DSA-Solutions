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

---
# Method 2 (BFS/Kahn's Algorithm)
```cpp
class Solution
{
	public:
	//Function to return list containing vertices in Topological order. 
	vector<int> topoSort(int V, vector<int> adj[]) {
	    vector<int> res;
	    vector<int> indegree(V, 0);
	    
	    for(int i = 0; i < V; i++) {
	        for(int nbr : adj[i]) {
	            indegree[nbr]++;
	        }
	    }
	    
	    queue<int> q;
	    for(int i = 0; i < V; i++) {
	        if(indegree[i] == 0) {
	            q.push(i);
	        }
	    }
	    
	    while(!q.empty()) {
	        int curr = q.front(); q.pop();
	        res.push_back(curr);
	        
	        for(int nbr : adj[curr]) {
	            indegree[nbr]--;
	            if(indegree[nbr] == 0) q.push(nbr);
	        }
	    }
	    
	    return res;
	}
};
```

Topological sorting is a linear ordering of vertices in a Directed Acyclic Graph (DAG) such that for every directed edge \( uv \), vertex \( u \) comes before \( v \) in the ordering. Kahn's algorithm is a popular method for finding a topological ordering of a DAG using BFS. Here's an explanation, time complexity analysis, space complexity analysis, and a dry run (ETSD) for the given code:

### Explanation (ETSD):

**E**: The code performs a topological sort using Kahn's algorithm (BFS-based approach):

1. **Initialization**:
   - `res`: A vector to store the result of the topological sort.
   - `indegree`: A vector of size `V` (number of vertices) initialized to `0`, to keep track of the in-degree (number of incoming edges) of each vertex.
   
2. **Calculate In-Degree**:
   - For each vertex, iterate over its adjacency list and increment the in-degree of each adjacent vertex.

3. **Queue Initialization**:
   - A queue `q` is initialized to keep track of vertices with an in-degree of `0` (i.e., vertices with no incoming edges). These vertices can be processed as they do not have any dependencies.

4. **BFS Process**:
   - While the queue is not empty:
     - Dequeue a vertex, add it to the `res` list.
     - For each neighboring vertex of the current vertex, decrement its in-degree. If the in-degree becomes `0`, enqueue it, as it can now be processed.

5. **Return Result**:
   - The `res` vector, which now contains the topological order of vertices, is returned.

### Time Complexity Analysis:

- **In-degree Calculation**: Iterating over all vertices and their adjacency lists takes \( O(V + E) \) time, where \( V \) is the number of vertices and \( E \) is the number of edges.
- **BFS Process**: Each vertex is enqueued and dequeued once, and each edge is processed once (for decrementing the in-degree). Hence, this also takes \( O(V + E) \) time.
  
Overall, the time complexity is **\( O(V + E) \)**.

### Space Complexity Analysis:

- `res` vector: Stores all `V` vertices, so it requires \( O(V) \) space.
- `indegree` vector: Also requires \( O(V) \) space.
- `queue` \( q \): In the worst case, all vertices could be in the queue at some point, requiring \( O(V) \) space.
- The adjacency list `adj[]` is given as input, but if we count its space, it is \( O(V + E) \).

Overall, the space complexity is **\( O(V + E) \)**, considering the input adjacency list.

### Dry Run (ETSD):

Consider a graph with 6 vertices and the following edges:

- Adjacency list representation:
  - `0 -> [2, 3]`
  - `1 -> [3, 4]`
  - `2 -> [3, 5]`
  - `3 -> []`
  - `4 -> [5]`
  - `5 -> []`

**Initial Setup**:
- `indegree = [0, 0, 0, 0, 0, 0]` (all in-degrees initialized to 0).
- Calculate in-degrees:
  - Vertex 0: `2, 3` → `indegree = [0, 0, 1, 1, 0, 0]`
  - Vertex 1: `3, 4` → `indegree = [0, 0, 1, 2, 1, 0]`
  - Vertex 2: `3, 5` → `indegree = [0, 0, 1, 3, 1, 1]`
  - Vertex 3: `[]` (no changes)
  - Vertex 4: `5` → `indegree = [0, 0, 1, 3, 1, 2]`
  - Vertex 5: `[]` (no changes)
- Queue initialization: `q = [0, 1]` (vertices with in-degree 0).
  
**BFS Process**:
1. `q = [0, 1]` → `res = []`
   - Dequeue `0`, add to `res` → `res = [0]`.
   - Process neighbors `2, 3`:
     - `indegree[2]--` → `indegree = [0, 0, 0, 3, 1, 2]`, enqueue `2`.
     - `indegree[3]--` → `indegree = [0, 0, 0, 2, 1, 2]`.

2. `q = [1, 2]` → `res = [0]`
   - Dequeue `1`, add to `res` → `res = [0, 1]`.
   - Process neighbors `3, 4`:
     - `indegree[3]--` → `indegree = [0, 0, 0, 1, 1, 2]`.
     - `indegree[4]--` → `indegree = [0, 0, 0, 1, 0, 2]`, enqueue `4`.

3. `q = [2, 4]` → `res = [0, 1]`
   - Dequeue `2`, add to `res` → `res = [0, 1, 2]`.
   - Process neighbors `3, 5`:
     - `indegree[3]--` → `indegree = [0, 0, 0, 0, 0, 2]`, enqueue `3`.
     - `indegree[5]--` → `indegree = [0, 0, 0, 0, 0, 1]`.

4. `q = [4, 3]` → `res = [0, 1, 2]`
   - Dequeue `4`, add to `res` → `res = [0, 1, 2, 4]`.
   - Process neighbor `5`:
     - `indegree[5]--` → `indegree = [0, 0, 0, 0, 0, 0]`, enqueue `5`.

5. `q = [3, 5]` → `res = [0, 1, 2, 4]`
   - Dequeue `3`, add to `res` → `res = [0, 1, 2, 4, 3]`.
   - No neighbors.

6. `q = [5]` → `res = [0, 1, 2, 4, 3]`
   - Dequeue `5`, add to `res` → `res = [0, 1, 2, 4, 3, 5]`.
   - No neighbors.

**Final Result**: `res = [0, 1, 2, 4, 3, 5]` is a valid topological order for the given graph.

This ETSD showcases how the code systematically processes vertices to produce a topological sort using Kahn's algorithm.

# Method 1 (DFS)
### Code
```cpp
class Solution {
  public:
    // Function to detect cycle in a directed graph.
    bool isCyclic(int V, vector<int> adj[]) {
        vector<int> visited(V, 0);
        vector<int> dfsVisited(V, 0);
        for(int i=0; i<V; i++) {
            if(!visited[i]) {
                if(dfs(i, visited, dfsVisited, adj)) {
                    return true;
                }
            }
        }
        return false;
    }
    
    bool dfs(int node, vector<int>& visited, vector<int>& dfsVisited, vector<int> adj[]) {
        visited[node]=1;
        dfsVisited[node]=1;
        
        for(auto& it:adj[node]) {
            if(!visited[it]) {
                if(dfs(it, visited, dfsVisited, adj)) {
                    return true;
                }
            } 
            else if(dfsVisited[it]&&visited[it]) {
                return true;
            }
        }
        
        dfsVisited[node]=0; // Unmark the node when backtracking
        return false;
    }
};
```
### Explanation

The code aims to detect a cycle in a directed graph using a Depth-First Search (DFS) approach. It maintains two vectors:

1. **`visited[]`**: This keeps track of all nodes that have been visited at least once.
2. **`dfsVisited[]`**: This keeps track of nodes that are currently in the current DFS call stack. It's used to detect back edges, which indicate cycles.

#### Function Breakdown

1. **`isCyclic()`**:
   - It initializes two vectors, `visited` and `dfsVisited`, both of size `V` (the number of vertices) and initializes them to 0.
   - It iterates through all vertices and, if a vertex hasn't been visited, it calls the `dfs()` function.
   - If `dfs()` returns `true` for any vertex, it means a cycle is detected, and the function returns `true`.
   - If no cycle is detected in any DFS traversal, it returns `false`.

2. **`dfs()`**:
   - This function performs the DFS traversal.
   - It marks the current node as visited in both `visited` and `dfsVisited`.
   - It iterates over all adjacent nodes:
     - If an adjacent node hasn't been visited, it recursively calls `dfs()` on that node.
     - If an adjacent node is already in the `dfsVisited` (i.e., part of the current path), it indicates a cycle, and the function returns `true`.
   - Once done with all adjacent nodes, it unmarks the current node from `dfsVisited` (backtracking) and returns `false`.

### Time Complexity Analysis

- **Time Complexity**: \(O(V + E)\)
  - Each vertex is processed once, and each edge is checked once in the adjacency list. Thus, the time complexity is linear in terms of the number of vertices \(V\) and edges \(E\).

### Space Complexity Analysis

- **Space Complexity**: \(O(V)\)
  - Two vectors of size \(V\) are used (`visited` and `dfsVisited`), resulting in an \(O(V)\) space complexity.
  - The recursion stack space in the worst case could go up to \(O(V)\) as well, but this does not affect the overall complexity.

### Dry Run

Let's perform a dry run with an example graph:

**Graph**:
- Vertices: 4 (0, 1, 2, 3)
- Edges: [(0, 1), (1, 2), (2, 0), (2, 3)]

**Adjacency List Representation**:
- `adj[0] = {1}`
- `adj[1] = {2}`
- `adj[2] = {0, 3}`
- `adj[3] = {}`

**Execution Flow**:

1. `isCyclic(4, adj)` is called.
2. Initializes `visited = [0, 0, 0, 0]` and `dfsVisited = [0, 0, 0, 0]`.
3. Iterates over each node:
   - **Node 0**:
     - Not visited, calls `dfs(0, visited, dfsVisited, adj)`.
     - Marks `visited[0] = 1`, `dfsVisited[0] = 1`.
     - Iterates over `adj[0] = {1}`:
       - **Node 1**:
         - Not visited, calls `dfs(1, visited, dfsVisited, adj)`.
         - Marks `visited[1] = 1`, `dfsVisited[1] = 1`.
         - Iterates over `adj[1] = {2}`:
           - **Node 2**:
             - Not visited, calls `dfs(2, visited, dfsVisited, adj)`.
             - Marks `visited[2] = 1`, `dfsVisited[2] = 1`.
             - Iterates over `adj[2] = {0, 3}`:
               - **Node 0** (Back to start, forms a cycle):
                 - Already visited and `dfsVisited[0] = 1` (indicates cycle).
                 - Returns `true` from `dfs(2)`.
             - Returns `true` from `dfs(1)`.
         - Returns `true` from `dfs(0)`.
   - **Node 0** cycle detected, so `isCyclic` returns `true`.

### Conclusion

The code correctly identifies the presence of a cycle in the directed graph using DFS and backtracking. The `visited` and `dfsVisited` vectors play crucial roles in tracking the state of nodes to identify cycles efficiently. The dry run confirms the functionality and accuracy of the approach.


# Method 2 (BFS)
### Code
```cpp
class Solution {
  public:
    // Function to detect cycle in a directed graph.
    bool isCyclic(int V, vector<int> adj[]) {
        vector<int> inedge(V, 0);  // Vector to store in-degrees of all vertices
        
        // Calculate in-degrees of all vertices
        for (int i = 0; i < V; i++) {
            for (auto j : adj[i]) {
                inedge[j]++;
            }
        }
        
        queue<int> q;
        
        // Enqueue all vertices with in-degree 0
        for (int i = 0; i < V; i++) {
            if (inedge[i] == 0) {
                q.push(i);
            }
        }
        
        int count = 0;  // To count the number of vertices processed
        
        while (!q.empty()) {
            int topNode = q.front();
            q.pop();
            count++;
            
            // Decrease the in-degree of adjacent vertices
            for (auto i : adj[topNode]) {
                inedge[i]--;
                
                // If in-degree becomes 0, enqueue the vertex
                if (inedge[i] == 0) {
                    q.push(i);
                }
            }
        }
        
        // If count is equal to V, no cycle is present
        // If count is less than V, cycle is present
        return count != V;
    }
};
```

### **Explanation**

The code is designed to detect cycles in a directed graph using Kahn's Algorithm, which is typically used for topological sorting. If the graph contains a cycle, Kahn's Algorithm will not be able to process all nodes, as some will remain with a non-zero in-degree.

**Steps Involved:**

1. **In-degree Calculation**:
   - The `inedge` vector is used to store the in-degree (the number of incoming edges) of each vertex. We iterate through each vertex and its adjacency list to populate the `inedge` vector.

2. **Queue Initialization**:
   - A queue `q` is initialized, and all vertices with an in-degree of 0 are enqueued. These vertices have no dependencies and can be processed immediately.

3. **Processing Nodes**:
   - The algorithm processes each node in the queue by popping it, incrementing a count of processed nodes (`count`), and reducing the in-degree of its adjacent nodes. If any adjacent node's in-degree drops to 0, it is enqueued.

4. **Cycle Detection**:
   - After processing, if the `count` of processed nodes equals the total number of vertices \( V \), the graph is acyclic (no cycle). If the `count` is less than \( V \), it indicates the presence of a cycle, as some nodes were not processed (due to remaining in-degrees).

### **Time Complexity Analysis**

- **In-degree Calculation**: 
  - This involves iterating over all vertices and their adjacency lists, resulting in a time complexity of \( O(V + E) \), where \( V \) is the number of vertices and \( E \) is the number of edges.

- **Queue Processing**: 
  - Each vertex is enqueued and dequeued at most once, and we examine each edge at most once when decrementing the in-degree of adjacent nodes. This adds another \( O(V + E) \) time complexity.

- **Overall Time Complexity**: 
  - Combining both steps, the overall time complexity is \( O(V + E) \).

### **Space Complexity Analysis**

- **Space for In-degree Array**: 
  - We use a vector `inedge` of size \( V \) to store in-degrees, requiring \( O(V) \) space.

- **Space for Queue**: 
  - The queue can hold up to \( V \) vertices, requiring \( O(V) \) space.

- **Space for Adjacency List**: 
  - The adjacency list representation of the graph requires \( O(V + E) \) space.

- **Overall Space Complexity**: 
  - Combining these, the overall space complexity is \( O(V + E) \).

### **Dry Run**

Let's perform a dry run using a sample directed graph to understand how the algorithm works:

#### **Graph Example**:
- Vertices (V) = 4
- Edges (E) = 4
- Adjacency List Representation:
  - 0 -> 1
  - 1 -> 2
  - 2 -> 3
  - 3 -> 1 (Cycle exists here)

#### **Initialization**:
- `inedge` = `[0, 0, 0, 0]`
  
#### **Step 1: Calculate In-degrees**:
- For vertex 0: `inedge` = `[0, 1, 0, 0]` (edge 0 -> 1)
- For vertex 1: `inedge` = `[0, 1, 1, 0]` (edge 1 -> 2)
- For vertex 2: `inedge` = `[0, 1, 1, 1]` (edge 2 -> 3)
- For vertex 3: `inedge` = `[0, 2, 1, 1]` (edge 3 -> 1, forms a cycle)

#### **Step 2: Initialize Queue**:
- Only vertex 0 has in-degree 0.
- `q` = `[0]`

#### **Step 3: Process Queue**:
- Pop vertex 0:
  - Count = 1
  - `inedge` after processing vertex 0: `[0, 1, 1, 1]`
- No other vertices have in-degree 0, so the queue remains empty.

#### **Cycle Detection**:
- Count = 1, which is less than the number of vertices \( V = 4 \). This indicates that a cycle is present in the graph because not all vertices could be processed.
- The function returns `true`, indicating a cycle is detected.

### **Summary**

- The algorithm uses Kahn's Algorithm to check if a directed graph has a cycle. It does this by maintaining the in-degree of each node and using a queue to process nodes with zero in-degrees. If all nodes are processed (i.e., the count equals \( V \)), the graph is acyclic. If not, the presence of a cycle is confirmed. The approach is efficient with a time complexity of \( O(V + E) \) and space complexity of \( O(V + E) \), making it suitable for large graphs.

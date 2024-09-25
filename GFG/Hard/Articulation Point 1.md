### C++ Code
```cpp
class Solution {
public:
    vector<int> articulationPoints(int V, vector<int> adj[]) {
        vector<int> inTime(V, -1), lowTime(V, -1);
        vector<bool> visited(V, false), isArticulation(V, false);
        int timer=0;
        
        // Helper DFS function
        function<void(int, int)> dfs=[&](int curr, int parent) {
            visited[curr]=true;
            inTime[curr]=lowTime[curr]=timer++;
            int childCount=0; // to count children for root node

            for (int neighbour:adj[curr]) {
                if (neighbour==parent) continue; // Ignore the edge to parent

                if (!visited[neighbour]) {
                    // Unvisited node, continue DFS
                    dfs(neighbour, curr);
                    childCount++;
                    lowTime[curr]=min(lowTime[curr], lowTime[neighbour]);

                    // Check articulation point condition
                    if (lowTime[neighbour] >= inTime[curr] && parent != -1) {
                        isArticulation[curr] = true;
                    }
                } 
                
                else {
                    // Already visited node
                    lowTime[curr]=min(lowTime[curr], inTime[neighbour]);
                }
            }

            // Special case for root
            if (parent==-1 && childCount>1) {
                isArticulation[curr]=true;
            }
        };

        // Start DFS from the first vertex (0 or any other unvisited node)
        for (int i=0; i<V; i++) {
            if (!visited[i]) {
                dfs(i, -1);
            }
        }

        // Collecting articulation points
        vector<int> result;
        for (int i=0; i<V; i++) {
            if (isArticulation[i]) {
                result.push_back(i);
            }
        }

        // Return sorted result or [-1] if empty
        if (result.empty()) return {-1};
        sort(result.begin(), result.end());
        return result;
    }
};
```
### 1. Explanation

The problem involves finding articulation points in a graph. An articulation point (or cut vertex) is a vertex which, when removed, makes the graph disconnected or increases the number of connected components. The provided solution uses Depth-First Search (DFS) with `inTime` and `lowTime` tracking to efficiently find these points.

**Key Concepts:**
- **inTime[v]**: The discovery time when vertex `v` is first visited.
- **lowTime[v]**: The lowest discovery time reachable from vertex `v` via its children, considering back edges to ancestors.
- **visited[v]**: Marks whether vertex `v` has been visited.
- **isArticulation[v]**: A boolean array to store if vertex `v` is an articulation point.

**Algorithm Steps:**
1. **Initialization**: Initialize `inTime`, `lowTime`, `visited`, and `isArticulation` arrays. Set a global `timer` to keep track of the discovery time.
2. **DFS Function**: Start DFS from an unvisited vertex:
   - Set the `inTime` and `lowTime` to the current `timer` value and increment `timer`.
   - For each unvisited neighbor, recursively call DFS.
   - Update the `lowTime` of the current vertex based on its children.
   - Check articulation conditions:
     - For non-root vertices: If a child `neighbour` has no back edge to any ancestor (`lowTime[neighbour] >= inTime[curr]`), mark `curr` as an articulation point.
     - For root vertex: If it has more than one child, mark it as an articulation point.
3. **Result Collection**: After DFS completes, collect all vertices marked as articulation points and return them sorted.

### 2. Time Complexity Analysis

- **DFS Traversal**: Each vertex and edge is processed once.
- **Updating `inTime` and `lowTime`**: Constant time operations for each vertex and edge.

**Time Complexity**: \(O(V + E)\), where \(V\) is the number of vertices, and \(E\) is the number of edges. This complexity is efficient and suitable for the given constraints (\(V, E \leq 10^5\)).

### 3. Space Complexity Analysis

- **Space for Arrays**: `inTime`, `lowTime`, `visited`, `isArticulation` each of size \(V\) — total space: \(O(V)\).
- **Stack Space for DFS**: Up to \(O(V)\) in the worst case (for recursion stack).

**Space Complexity**: \(O(V)\), which is manageable within the problem's constraints.

### 4. Dry Run

**Graph**:  
```
Vertices: 0, 1, 2, 3, 4  
Edges: (0-1), (1-2), (1-3), (3-4)  
Adjacency List:
0: [1]
1: [0, 2, 3]
2: [1]
3: [1, 4]
4: [3]
```

**Expected Articulation Points**: `1` and `3`

**Step-by-Step Execution:**

1. **Initialization**:
   - `inTime[] = {-1, -1, -1, -1, -1}`
   - `lowTime[] = {-1, -1, -1, -1, -1}`
   - `visited[] = {false, false, false, false, false}`
   - `isArticulation[] = {false, false, false, false, false}`
   - `timer = 0`

2. **Start DFS from Vertex 0**:
   - `dfs(0, -1)`
   - `visited[0] = true`
   - `inTime[0] = lowTime[0] = 0`
   - `timer = 1`
   - `childCount = 0`
   - Visit Neighbor 1 from Vertex 0:
     - `dfs(1, 0)`
     - `visited[1] = true`
     - `inTime[1] = lowTime[1] = 1`
     - `timer = 2`
     - `childCount = 0`
     
3. **Continue DFS from Vertex 1**:
   - Visit Neighbor 0:
     - Already visited and is parent, so continue.
   - Visit Neighbor 2 from Vertex 1:
     - `dfs(2, 1)`
     - `visited[2] = true`
     - `inTime[2] = lowTime[2] = 2`
     - `timer = 3`
     - `childCount = 0`
   - Visit Neighbor 1:
     - Already visited and is parent, so continue.
   - Return to Vertex 1 from 2:
     - `lowTime[1] = min(lowTime[1], lowTime[2]) = min(1, 2) = 1`
     - `childCount = 1`
   - Visit Neighbor 3 from Vertex 1:
     - `dfs(3, 1)`
     - `visited[3] = true`
     - `inTime[3] = lowTime[3] = 3`
     - `timer = 4`
     - `childCount = 0`
     
4. **Continue DFS from Vertex 3**:
   - Visit Neighbor 1:
     - Already visited and is parent, so continue.
   - Visit Neighbor 4 from Vertex 3:
     - `dfs(4, 3)`
     - `visited[4] = true`
     - `inTime[4] = lowTime[4] = 4`
     - `timer = 5`
     - `childCount = 0`
   - Visit Neighbor 3:
     - Already visited and is parent, so continue.
   - Return to Vertex 3 from 4:
     - `lowTime[3] = min(lowTime[3], lowTime[4]) = min(3, 4) = 3`
     - `childCount = 1`
     - Check articulation condition for Vertex 3:
       - `lowTime[4] >= inTime[3]` (4 >= 3): True
       - Vertex 3 is not the root (parent is 1).
       - `isArticulation[3] = true`.

5. **Return to Vertex 1 from 3**:
   - `lowTime[1] = min(lowTime[1], lowTime[3]) = min(1, 3) = 1`
   - `childCount = 2`
   - Check articulation condition for Vertex 1:
     - `lowTime[3] >= inTime[1]` (3 >= 1): True
     - Vertex 1 is not the root (parent is 0).
     - `isArticulation[1] = true`.

6. **Return to Vertex 0 from 1**:
   - `lowTime[0] = min(lowTime[0], lowTime[1]) = min(0, 1) = 0`
   - `childCount = 1` (Only one child directly connected)
   
7. **Special Case Check for Root (Vertex 0)**:
   - Root (0) has only 1 child in DFS tree, so it is **not** an articulation point.

### Final Results

- `isArticulation[] = {false, true, false, true, false}`
- Articulation points: `1` and `3`.

This detailed dry run illustrates the correctness and efficiency of the algorithm, confirming that it accurately identifies the articulation points in the graph.

### Code:
```cpp
class Solution {
public:
    void dfs(int node, unordered_map<int,bool> &vis, stack<int>&st, vector<vector<int>>& adj){
        vis[node]=true;
        for(auto neigh:adj[node]){
            if(!vis[neigh]){
                dfs(neigh, vis, st, adj);
            }
        }
        st.push(node);
    }
    
    void revofdfs(int node, unordered_map<int,bool> &vis, unordered_map<int,list<int>> & adj){
        vis[node] = true;
        for(auto neigh:adj[node]){
            if(!vis[neigh]){
                revofdfs(neigh, vis, adj);
            }
        }
    }
    
    int kosaraju(int V, vector<vector<int>>& adj){
        stack<int>st;
        unordered_map<int,bool>vis;
        for(int i=0; i<V; i++){
            if(!vis[i]){
                dfs(i, vis, st, adj);
            }
        }
        
        //transpose the graph because element in stack in reverse order
        unordered_map<int,list<int>>transpose;
        for(int i=0; i<V; i++){
            vis[i] = false; 
            for(auto neigh:adj[i]){
                transpose[neigh].push_back(i);
            }
        }
        
        //now count all strong connected components using dfs
        int count =0;
        while(!st.empty()){
            int top = st.top();
            st.pop();
            if(!vis[top]){
                revofdfs(top, vis, transpose);
                count++;
            }
        }
        return count;
    
    }
};
```
### Explanation of the Code

The given code implements Kosaraju's algorithm, which is used to find the number of Strongly Connected Components (SCCs) in a directed graph. A strongly connected component is a maximal subgraph where every vertex is reachable from every other vertex in the same subgraph. Kosaraju's algorithm efficiently finds all SCCs in a graph using two passes of Depth First Search (DFS).

Here's how the code works:

1. **DFS to Fill Stack (`dfs` function)**: The first DFS is used to fill the stack with vertices in the order of their finishing times. The idea is to perform DFS on the original graph and push nodes to a stack after exploring all their neighbors (post-order).

2. **Transpose Graph**: After the first DFS, the graph is transposed (i.e., all edges are reversed). This is stored in the `transpose` map.

3. **DFS on Transposed Graph (`revofdfs` function)**: The second DFS is performed on the transposed graph using the order of nodes in the stack. Nodes are popped from the stack, and a DFS is performed if they have not been visited. Each DFS call on the transposed graph discovers one SCC.

4. **Counting SCCs**: The count of SCCs is maintained using the `count` variable, which is incremented every time a new DFS traversal starts on the transposed graph.

### Time Complexity

- **First DFS Pass**: \(O(V + E)\), where \(V\) is the number of vertices and \(E\) is the number of edges. Each node and edge is processed once.
- **Transposing the Graph**: \(O(V + E)\), since each edge is reversed, and each node is visited.
- **Second DFS Pass**: \(O(V + E)\), similar to the first pass.

Overall time complexity: \(O(V + E)\).

### Space Complexity

- **Stack for storing order**: \(O(V)\)
- **Visited map**: \(O(V)\)
- **Transpose graph storage**: \(O(V + E)\)

Overall space complexity: \(O(V + E)\).

### Dry Run of the Code

Let's perform a dry run of the code using a simple graph:

**Graph:**
```
0 → 1 → 2 → 0
3 → 4
4 → 5 → 3
```

This graph has two strongly connected components: `{0, 1, 2}` and `{3, 4, 5}`.

1. **First DFS pass to fill the stack:**
   - Start from node `0`: Visit `0 → 1 → 2`, then push `2`, `1`, and `0` to the stack in that order.
   - Node `3`: Visit `3 → 4 → 5`, then push `5`, `4`, and `3` to the stack.

   **Stack after first pass:** `[3, 4, 5, 0, 1, 2]`.

2. **Transpose the graph:**
   - Original edges: `0→1, 1→2, 2→0, 3→4, 4→5, 5→3`
   - Transposed edges: `1→0, 2→1, 0→2, 4→3, 5→4, 3→5`

   **Transposed graph:**
   ```
   1 → 0 → 2 → 1
   4 → 3 → 5 → 4
   ```

3. **Second DFS pass using the stack order:**
   - Pop `3` from the stack and perform DFS: Visits `3 → 4 → 5`, marking this as one SCC. Count becomes `1`.
   - Pop `0` from the stack and perform DFS: Visits `0 → 2 → 1`, marking this as another SCC. Count becomes `2`.

4. **Final count:** The number of SCCs is `2`.

### Conclusion

Kosaraju's algorithm effectively identifies the SCCs of a directed graph by leveraging two DFS passes and graph transposition. The implementation ensures that all vertices are processed in an optimal manner, guaranteeing that each SCC is counted correctly.

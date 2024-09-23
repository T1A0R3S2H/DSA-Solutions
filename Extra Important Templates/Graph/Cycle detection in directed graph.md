## DFS (Depth first search)
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
---
 
## BFS (similar to Kahn's Algorithm)
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

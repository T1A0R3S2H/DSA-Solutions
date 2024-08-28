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

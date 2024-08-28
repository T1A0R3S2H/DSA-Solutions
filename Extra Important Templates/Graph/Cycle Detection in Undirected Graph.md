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

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

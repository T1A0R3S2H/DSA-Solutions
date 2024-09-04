```cpp
class Solution {
  public:
    /*  Function to implement Bellman Ford
    *   edges: vector of vectors which represents the graph
    *   S: source vertex to start traversing graph with
    *   V: number of vertices
    */
    vector<int> bellman_ford(int V, vector<vector<int>>& edges, int S) {
        vector<int> dist(V, 1e8);
        dist[S] = 0;
        
        // run n-1 times
        for(int i=1; i<=V-1; i++){
            for(auto it:edges){
                if(dist[it[0]]!=1e8 && dist[it[0]]+it[2] < dist[it[1]]){
                    dist[it[1]]=dist[it[0]]+it[2];
                }
            }
        }
        
        // check for -ve cycles
        for(auto it:edges){
            if(dist[it[0]]!=1e8 && dist[it[0]]+it[2] < dist[it[1]]){
                return {-1};
            }
        }
        return dist;
    }
};
```

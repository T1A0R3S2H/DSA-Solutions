### Code
```cpp
class Solution {
  public:
  
    void dfs(vector<vector<int>>& arr, int curr, vector<bool>&visited){
        visited[curr]=true;
        for(int i=0;i<arr[curr].size();i++){
            if(arr[curr][i]==1 && !visited[i]){
                dfs(arr, i, visited);
            }
        }
    }
    
    int numProvinces(vector<vector<int>> adj, int V) {
        vector<bool> visited(V, false);
        int count=0;
        for(int i=0;i<V;i++){
            if(!visited[i]){
                count++;
                dfs(adj, i, visited);
            }
        }
        return count;
    }
    
};
```

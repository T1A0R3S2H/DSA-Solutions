# Undirected graph
```cpp
vector<int>shortestPath(vector<pair<int, int>adges, int n, int m, int s, int t){
  // adj list
  unordered_map<int, list<int>>adj;
  for(int i=0;<edges.size();i++){
    int u=edges[i].first;
    int v=edges[i].second;
    adj[u].push_back(v);
    adj[v].push_bacck(v);
  }

  // bfs
  unordered_map<int, bool>visited;
  unordered_map<int, int> parent;
  queue<int>q;
  visited[s]=true;
  while(!q.empty()){
    int front=q.front();
    q.pop();
    for(auto i:adj[front]){
      if(!visited[i]){
        visited[i]=true;
        parent[i]=front;
        q.push(i);
      }
    }
  }

  // return the ans
  vector<int> ans;
  int currNode=t;
  ans.push_back(t);
  while(currNode!=s){
    currNode=parent[currNode];
    ans.push_back(currNode);
  }
}
```


# Directed Acyclic Graph (DAG)
```cpp

```

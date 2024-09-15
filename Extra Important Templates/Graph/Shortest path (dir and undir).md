# Undirected graph
```c
vector<int>shortestPath(vector<pair<int, int>edges, int n, int m, int s, int t){
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
---

# Directed Acyclic Graph (DAG)
```c
#include<bits/stdc++.h>
void toposort(int node, vector<pair<int, int>> adj[], vector<int>& vis, stack<int>& st) {
    vis[node] = 1;
    for (auto it : adj[node]) {
        if (!vis[it.first]) { // Check it.first instead of it
            toposort(it.first, adj, vis, st);
        }
    }
    st.push(node);
}

 

vector<int> shortestPathInDAG(int n, int m, vector<vector<int>> &edges) {
// Creating graph

    vector<pair<int, int>> adj[n];
    for (int i=0; i<m; i++) {
        int u=edges[i][0];
        int v=edges[i][1];
        int wt=edges[i][2];

        adj[u].push_back({v, wt});
    }

 

// Topological sorting
stack<int> st;
vector<int> vis(n, 0);

// Perform topological sort for all components
for (int i=0; i<n; i++) {
    if (!vis[i]) {
        toposort(i, adj, vis, st);
    }

}

// Initialize distance vector to store the shortest path distances
vector<int> dist(n, 1e9);

// Assuming the source node is 0 (you may modify it as per your requirement)
dist[0]=0;

// Process nodes in topological order
while (!st.empty()) {
    int node = st.top();
    st.pop();
    if (dist[node] != 1e9) { // Only proceed if the current node has been reached
        for (auto it : adj[node]) {
            int v = it.first;
            int wt = it.second;
            if (dist[node]+wt < dist[v]) {
                dist[v]=dist[node]+wt;
            }
        }
    }

}

// Replace unreachable nodes' distances with -1
for (int i = 0; i < n; i++) {
    if (dist[i]==1e9) dist[i]=-1;
}

return dist;
}
```

---




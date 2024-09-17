# DFS (Depth first search):
```cpp
class Solution {
	public:
	void dfs(int start, vector<bool> &visited, stack<int> &s, vector<int> adj[]) {
	    visited[start]=1;
	    for(auto& it:adj[start]) {
	        if(!visited[it]) {
	            dfs(it, visited, s, adj);
	        }
	    }
	    s.push(start);
	}
	
	vector<int> topoSort(int V, vector<int> adj[]) {
	    vector<bool> visited(V);
	    stack<int> s;
	    for(int i=0; i<V; i++) {
	        if(!visited[i]) {
	            dfs(i, visited, s, adj);
	        }
	    }
	    vector<int> res;
	    for(int i=0; i<V; i++) {
	        res.push_back(s.top()); 
	        s.pop();
	    }
	    return res;
	}
};
```
---
# BFS (Breadth first search):
```cpp
vector<int>topo(int v, vector<int>adj[]){
	vector<int> res;
	vector<int>indgree(v, 0);
	for(int i=0;i<v;i++){
		for(int it:adj[i]){
			indegree[it];
		}
	}
	queue<int> q;
	for(int i=0;i<v;i++){
		if(indegree==0){
			q.push(i);
		}
	}
	while(!q.empty()){
		int curr=q.front();
		q.pop();
		res.push_back(curr);
		for(int x:adj[curr]){
			indegree[x]--;
			if(indegree[x]==0){
				q.push_back(x);
			}
		}
	return res;
	}
}
```

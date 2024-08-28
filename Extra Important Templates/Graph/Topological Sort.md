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

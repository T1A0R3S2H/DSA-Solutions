# Method 1 (Using Min-Heap)
```cpp
vector<int> dijkstra(int V, vector<vector<int>> adj[], int S) {
        vector<int> dist(V, INT_MAX);
        dist[S]=0;
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
        pq.push({0, S});
    
        while (!pq.empty()) {
            int d=pq.top().first;
            int node=pq.top().second;
            pq.pop();
            for (auto it:adj[node]) {
                int neighbor=it[0];
                int weight=it[1];
                if (dist[node]+weight<dist[neighbor]) {
                    dist[neighbor]=dist[node]+weight;
                    pq.push({dist[neighbor], neighbor});
                }
            }
        }
    
        return dist;
    }
```

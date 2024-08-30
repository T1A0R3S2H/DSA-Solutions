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
# Method 2 (Using Set)
```cpp
#include <vector>
#include <set>
#include <limits.h>

using namespace std;

vector<int> dijkstra(int V, vector<vector<int>> adj[], int S) {
    // Initialize distances to all vertices as infinite, except for the source
    vector<int> dist(V, INT_MAX);
    dist[S] = 0;

    // Set to process vertices with the current shortest known distance
    set<pair<int, int>> st;
    
    // Insert source with distance 0
    st.insert({0, S});

    while (!st.empty()) {
        // Extract the vertex with the smallest distance
        auto it = st.begin();
        int d = it->first; // current distance
        int node = it->second; // current node
        st.erase(it);

        // Iterate over all the neighbors of the current node
        for (auto it : adj[node]) {
            int neighbor = it[0];
            int weight = it[1];

            // Check if a shorter path to the neighbor is found
            if (dist[node] + weight < dist[neighbor]) {
                // If the neighbor already has a distance value, remove it from the set
                if (dist[neighbor] != INT_MAX) {
                    st.erase({dist[neighbor], neighbor});
                }

                // Update the shortest distance and insert the new pair into the set
                dist[neighbor] = dist[node] + weight;
                st.insert({dist[neighbor], neighbor});
            }
        }
    }

    return dist;
}
```

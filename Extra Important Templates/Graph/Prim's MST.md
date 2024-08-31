# Prim's Algorithm Code for Minimum Spanning Tree

## Assumption:
The implementation of Prim's algorithm assumes the graph is an undirected, connected, and weighted graph with non-negative edge weights, where vertices are numbered from 1 to `n`. It also assumes the input graph is provided as a vector of edges, and there are no self-loops or multiple edges between the same pair of vertices. The algorithm starts constructing the MST from vertex 1 and expects the graph to be connected, ensuring all vertices are reachable.
## Code:
```cpp
#include <bits/stdc++.h>
using namespace std;

vector<pair<pair<int, int>, int>> calculatePrimsMST(int n, int m, vector<pair<pair<int, int>, int>> &g) {
    // Adjacency list to represent the graph
    unordered_map<int, list<pair<int, int>>> adj;

    // Building the adjacency list from the given edges
    for (int i = 0; i < g.size(); i++) {
        int u = g[i].first.first;  // Start node of the edge
        int v = g[i].first.second; // End node of the edge
        int w = g[i].second;       // Weight of the edge
        adj[u].push_back(make_pair(v, w)); // Add edge u -> v
        adj[v].push_back(make_pair(u, w)); // Add edge v -> u (since the graph is undirected)
    }

    // Key values used to pick minimum weight edge in cut
    vector<int> key(n + 1, INT_MAX);  // Initialize all keys to infinity
    vector<int> mst(n + 1, false);    // To track vertices included in MST
    vector<int> parent(n + 1, -1);    // Array to store constructed MST

    key[1] = 0; // Start with the first vertex, set key value to 0

    // MST will have n-1 edges, so we run the loop n-1 times
    for (int i = 1; i < n; i++) {
        int mini = INT_MAX;
        int u;

        // Find the vertex with the minimum key value, which is not yet included in MST
        for (int v = 1; v <= n; v++) {
            if (key[v] < mini && mst[v] == false) {
                mini = key[v];
                u = v;
            }
        }

        mst[u] = true; // Include vertex u in MST

        // Update key values and parent index of the adjacent vertices of the picked vertex
        for (auto neighbour : adj[u]) {
            int v = neighbour.first; // Adjacent vertex
            int w = neighbour.second; // Weight of edge u-v

            // Update key only if v is not in MST and weight of u-v is smaller than current key of v
            if (mst[v] == false && w < key[v]) {
                parent[v] = u;
                key[v] = w;
            }
        }
    }

    // Construct the result MST
    vector<pair<pair<int, int>, int>> answer;
    for (int i = 2; i <= n; i++) {
        answer.push_back(make_pair(make_pair(parent[i], i), key[i]));
    }

    return answer;
}
```
## Time and Space Complexities: 
**`TC`: `O(n^2)`; min key vertex - O(n^2) + adj[] - O(m) + update and parent - O(m)**

**`SC`: `O(n+m)`; m:vertices and n:edges; adj[] - O(n+m) + key/mst/parent - O(m)**

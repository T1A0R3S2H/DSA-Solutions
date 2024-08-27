```cpp
class Solution {
  public:
    // Function to return a list containing the DFS traversal of the graph.
    void dfs(int node, vector<int>& visited, vector<int>& ans, vector<int> adj[]) {
        visited[node] = 1;        // Mark the current node as visited
        ans.push_back(node);      // Store the node in the result

        // Traverse all the adjacent nodes
        for (auto& it : adj[node]) {
            if (!visited[it]) {   // If the adjacent node is not visited
                dfs(it, visited, ans, adj); // Recursively call DFS on the adjacent node
            }
        }
    }

    // Function to return Depth First Traversal of given graph.
    vector<int> dfsOfGraph(int V, vector<int> adj[]) {
        vector<int> ans;          // To store the DFS traversal
        vector<int> visited(V, 0); // To track visited nodes

        // Starting DFS traversal from node 0
        dfs(0, visited, ans, adj);

        return ans;               // Return the DFS traversal result
    }
    
    
};
```

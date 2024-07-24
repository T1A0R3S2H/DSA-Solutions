### Code
```cpp
class Solution
{
    public:
    // Function to return a list of nodes visible from the top view 
    // from left to right in Binary Tree.
    vector<int> topView(Node *root)
    {
        map<int,int> v; // Map to store vertical height index and node data

        vector<int> a; // Vector to store the result
        if(!root) return a; // If the root is null, return an empty vector

        queue<pair<Node*,int>> q; // Queue to perform level order traversal
        q.push({root,0}); // Initialize the queue with the root node and its vertical height (0)
 
        while(q.size()){
            Node *t = q.front().first; // Get the front node
            int vh = q.front().second; // Get the vertical height of the front node
            q.pop(); // Remove the front node from the queue
            
            // If this column index already has a node, we don't need to change it 
            if(v[vh]==0) v[vh]=t->data; // If this vertical height is not in the map, add it 
            
            if(t->left) q.push({t->left,vh-1}); // Push the left child with vertical height vh-1
            if(t->right) q.push({t->right,vh+1}); // Push the right child with vertical height vh+1
        }
        
        // All the nodes in the map are the nodes which are 
        // encountered first in the vertical traversal so they give us the top view of the tree
        for(auto x:v) a.push_back(x.second); // Add the nodes to the result vector
        
        return a; // Return the result
    }
};
```

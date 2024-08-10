```cpp
class Solution
{
public:
    Node *flattenBST(Node *root)
    {
        if (root == NULL) return NULL;  // Return NULL if root is NULL
        vector<Node*> nodes;
        inorder(root, nodes);
        
        int n = nodes.size();
        for (int i = 0; i < n-1; ++i) {
            nodes[i]->left = NULL;
            nodes[i]->right = nodes[i+1];
        }
        
        nodes[n-1]->left = NULL;
        nodes[n-1]->right = NULL;

        return nodes[0];
    }

    void inorder(Node* root, vector<Node*>& nodes) {
        if (root == NULL) return;
        inorder(root->left, nodes);
        nodes.push_back(root);
        inorder(root->right, nodes);
    }
};
```

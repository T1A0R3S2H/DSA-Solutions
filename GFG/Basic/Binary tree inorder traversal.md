```cpp
class Solution {
  public:
    // Function to return a list containing the inorder traversal of the tree.
    vector<int> inOrder(Node* root) {
        vector<int> result;
        inorder(root, result);
        return result;
    }
    
    void inorder(Node* node, vector<int>& result) {
        if (node!=nullptr) {
            inorder(node->left, result);
            result.push_back(node->data);
            inorder(node->right, result);
        }
    }
};
```

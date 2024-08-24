```cpp
class Solution{
    public:
    // return true/false denoting whether the tree is Symmetric or not
    bool isMirror(Node* left, Node* right) {
        if (!left && !right) return true;
        if (!left || !right) return false;
        return (left->data==right->data) &&
               isMirror(left->left, right->right) &&
               isMirror(left->right, right->left);
    }

    bool isSymmetric(struct Node* root) {
        if (!root) return true;
        return isMirror(root->left, root->right);
    }
    
    
    
    
};
```

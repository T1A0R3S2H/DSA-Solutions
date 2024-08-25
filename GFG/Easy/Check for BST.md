```cpp
class Solution {
  public:
    // Function to check whether a Binary Tree is BST or not.
    bool isBST(Node* root) {
        vector<int> ans;
        inorder(root, ans);
        int n=ans.size();
        for (int i=1;i<n;i++) {
            if (ans[i-1]>=ans[i]) {
                return false;
            }
        }
        return true;
    }
    
    void inorder(Node* root, vector<int>& ans) {
        if (root==nullptr) return;
        inorder(root->left, ans);
        ans.push_back(root->data);
        inorder(root->right, ans);
    }
};
```

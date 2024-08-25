```cpp
class Solution {
  public:
    // Return the Kth smallest element in the given BST
    int KthSmallestElement(Node *root, int K) {
        vector<int> ans;
        inorder(root, ans);
        int n=ans.size();
        if (K>0 && K<=n) {
            return ans[K-1];
        } 
        else {
            return -1; // Return -1 if K is not valid
        }
    }
    
    void inorder(Node* root, vector<int>& ans) {
        if (root==nullptr) return;
        inorder(root->left, ans);
        ans.push_back(root->data);
        inorder(root->right, ans);
    }
};
```

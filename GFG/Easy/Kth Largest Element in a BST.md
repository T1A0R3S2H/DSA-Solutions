```cpp
class Solution {
    public:
    int kthLargest(Node *root, int K) {
        vector<int> ans;
        inorder(root, ans);
        int n=ans.size();
        return ans[n-K];
    }
    void inorder(Node* root, vector<int>& ans) {
        if (root==nullptr) return;
        inorder(root->left, ans);
        ans.push_back(root->data);
        inorder(root->right, ans);
    }
};
```

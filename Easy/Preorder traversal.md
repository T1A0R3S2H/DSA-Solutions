```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        pre(root, res);
        return res;
    }

    void pre(TreeNode* root, vector<int>& res){
        if (root != NULL) {
            res.push_back(root->val);  
            pre(root->left, res);  
            pre(root->right, res);  
        }
    }
};
```

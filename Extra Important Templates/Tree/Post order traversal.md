```cpp
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> res;
        post(root, res);
        return res;
    }


    void post(TreeNode* root, vector<int>& res){
        if (root != NULL) {  
            post(root->left, res);  
            post(root->right, res);
            res.push_back(root->val);
        }
    }
};
```

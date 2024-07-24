### Code
```cpp
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        vector<int> res;
        if(root == NULL) return res;
        dfs(root, 0, res);
        return res;
    }
    
    void dfs(TreeNode* node, int level, vector<int>& res) {
        if(node == NULL) return;
        if(res.size() == level) res.push_back(node->val);
        dfs(node->right, level+1, res);
        dfs(node->left, level+1, res);
    }
};
```

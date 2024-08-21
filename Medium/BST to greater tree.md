```cpp
class Solution {
public:
    TreeNode* convertBST(TreeNode* root) {
        int sum=0;
        
        function<void(TreeNode*)> dfs = [&](TreeNode* node) {
            if (!node) return;
            dfs(node->right);
            sum+=node->val;
            node->val=sum;
            dfs(node->left);
        };
        
        dfs(root);
        return root;
    }
};
```

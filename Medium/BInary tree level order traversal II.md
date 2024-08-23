```cpp
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector<vector<int>> result;
        if (root==nullptr) {
            return result;
        }
        queue<TreeNode*> q;
        q.push(root);
        
        while (!q.empty()) {
            int n=q.size();
            vector<int> currentLevel;
            
            for (int i=0; i<n; i++) {
                TreeNode* node=q.front();
                q.pop();
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
                currentLevel.push_back(node->val);
            }
            result.push_back(currentLevel);
        }
        reverse(result.begin(), result.end());
        return result;
    }
};
```

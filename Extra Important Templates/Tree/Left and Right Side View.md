## Left Side View
```cpp
class Solution {
public:
    void dfs(Node* node, int level, vector<int>& res) {
        if(node == NULL) return;
        if(res.size() == level) res.push_back(node->data);
        dfs(node->left, level+1, res);
        dfs(node->right, level+1, res);
    }

    vector<int> leftView(Node *root) {
        vector<int> res;
        if(root == NULL) return res;
        dfs(root, 0, res);
        return res;
    }
};
```

## Right Side View
```cpp
class Solution {
public:
    void dfs(TreeNode* node, int level, vector<int>& res) {
        if(node == NULL) return;
        if(res.size() == level) res.push_back(node->val);
        dfs(node->right, level+1, res);
        dfs(node->left, level+1, res);
    }

    vector<int> rightSideView(TreeNode* root) {
        vector<int> res;
        if(root == NULL) return res;
        dfs(root, 0, res);
        return res;
    }
};
```

```cpp
int findBottomLeftValue(TreeNode* root) {
        if (!root) return 0;
        queue<TreeNode*> q;
        q.push(root);
        TreeNode* curr=nullptr;
        while (!q.empty()) {
            curr=q.front();
            q.pop();
            
            if (curr->right) q.push(curr->right);
            if (curr->left) q.push(curr->left);
        }
        return curr->val;
    }
```

- BFS, and in the queue, first right then left to ensure that the leftmost will be the last node.
- TC: O(n), This is because we visit each node exactly once during the level-order traversal.
- SC: O(n/2) i.e. O(n), In the worst case, the queue may contain all nodes in the last level of the binary tree. For a complete binary tree, the last level can contain up to n/2 nodes, which is in the order of O(n).

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int pathSum(TreeNode* root, int targetSum) {
        vector<int> path;
        int count = 0;
        solve(root, targetSum, count, path);
        return count;
    }

    void solve(TreeNode *root, int targetSum, int &count, vector<int> &path) {
        // base case
        if (root == NULL) return;
        path.push_back(root->val);
        // left path
        solve(root->left, targetSum, count, path);
        // right path
        solve(root->right, targetSum, count, path);
        // check ksum
        int n = path.size();
        long long sum = 0;  // Change the type of sum to long long
        for (int i = n - 1; i >= 0; i--) {
            sum += path[i];
            if (sum == targetSum) count++;
        }
        // backtrack: remove the current node from the path
        path.pop_back();
    }
};

```

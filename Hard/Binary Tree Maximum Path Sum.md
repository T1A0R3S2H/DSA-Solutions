```cpp
class Solution {
public:
    int maxPathSum(TreeNode* root) {
        int maxSum = INT_MIN;
        maxPathSumHelper(root, maxSum);
        return maxSum;
    }

private:
    int maxPathSumHelper(TreeNode* node, int &maxSum) {
        if (node == nullptr) {
            return 0;
        }
        
        int leftMax = max(maxPathSumHelper(node->left, maxSum), 0);
        int rightMax = max(maxPathSumHelper(node->right, maxSum), 0);
        int currentMaxPath = node->val + leftMax + rightMax;
        maxSum = max(maxSum, currentMaxPath);
        
        return node->val + max(leftMax, rightMax);
    }
};
```


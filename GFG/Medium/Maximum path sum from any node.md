```cpp
class Solution {
  public:
    //Function to return maximum path sum from any node in a tree.
    int findMaxSum(Node* root)
    {
        int maxSum=INT_MIN;
        solve(root, maxSum);
        return maxSum;
    }
    
    int solve(Node* node, int &maxSum) {
        if (node==nullptr) {
            return 0;
        }
        // only non -ve values are considered (so that sum doesn't decrease)
        int leftMax=max(solve(node->left, maxSum), 0);
        int rightMax=max(solve(node->right, maxSum), 0);
        int currentMaxPath=node->data+leftMax+rightMax;
        maxSum=max(maxSum, currentMaxPath);
        return node->data+max(leftMax, rightMax);
    }
};
```

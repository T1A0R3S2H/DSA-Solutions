```cpp
class Solution {
public:
    int maxPathSum(TreeNode* root) {
        int maxSum = INT_MIN;
        solve(root, maxSum);
        return maxSum;
    }

private:
    int solve(TreeNode* node, int &maxSum) {
        if (node == nullptr) {
            return 0;
        }
        // only non -ve values are considered (so that sum doesn't decrease)
        int leftMax = max(maxPathSumHelper(node->left, maxSum), 0);
        int rightMax = max(maxPathSumHelper(node->right, maxSum), 0);
        int currentMaxPath = node->val + leftMax + rightMax;
        maxSum = max(maxSum, currentMaxPath);
        
        return node->val + max(leftMax, rightMax);
    }
};
```

### Explanation
This solution calculates the maximum path sum in a binary tree using a recursive helper function. Here are the key components and their roles:

1. **maxPathSum**:
   - Initializes `maxSum` to `INT_MIN` to ensure any valid path sum will be larger.
   - Calls the helper function `solve` to start the recursion.
   - Returns the maximum path sum found.

2. **solve**:
   - Takes the current node and a reference to `maxSum`.
   - If the node is `nullptr`, returns 0 because there's no path.
   - Recursively calculates the maximum path sum for the left and right children, considering only non-negative contributions (using `max(..., 0)`).
   - Computes `currentMaxPath` as the sum of the node's value plus the maximum path sums of its left and right children.
   - Updates `maxSum` to be the larger of `maxSum` and `currentMaxPath`.
   - Returns the node's value plus the maximum of the left or right path sums to propagate the path sum upwards.

### Time Complexity (TC)
- **O(N)**: The function visits each node exactly once, where \( N \) is the number of nodes in the tree.

### Space Complexity (SC)
- **O(H)**: The space complexity is determined by the depth of the recursion stack, which is proportional to the height \( H \) of the tree. In the worst case, \( H = N \) (e.g., a completely unbalanced tree), but on average for balanced trees, \( H = \log N \).

### Dry Run

Let's perform a dry run with the example tree: `[-10,9,20,null,null,15,7]`.

1. **Tree Structure**:
   ```
       -10
       /  \
      9   20
         /  \
        15   7
   ```

2. **Initialization**:
   - `maxSum = INT_MIN`

3. **Recursive Calls**:

   - Call `solve(root)` where `root` is `-10`:
     - Call `solve(-10->left)` where left is `9`:
       - Call `solve(9->left)` which is `nullptr`, return `0`
       - Call `solve(9->right)` which is `nullptr`, return `0`
       - Calculate `currentMaxPath` for `9`: `9 + 0 + 0 = 9`
       - Update `maxSum`: `max(INT_MIN, 9) = 9`
       - Return: `9 + max(0, 0) = 9`
     - Call `solve(-10->right)` where right is `20`:
       - Call `solve(20->left)` where left is `15`:
         - Call `solve(15->left)` which is `nullptr`, return `0`
         - Call `solve(15->right)` which is `nullptr`, return `0`
         - Calculate `currentMaxPath` for `15`: `15 + 0 + 0 = 15`
         - Update `maxSum`: `max(9, 15) = 15`
         - Return: `15 + max(0, 0) = 15`
       - Call `solve(20->right)` where right is `7`:
         - Call `solve(7->left)` which is `nullptr`, return `0`
         - Call `solve(7->right)` which is `nullptr`, return `0`
         - Calculate `currentMaxPath` for `7`: `7 + 0 + 0 = 7`
         - Update `maxSum`: `max(15, 7) = 15`
         - Return: `7 + max(0, 0) = 7`
       - Calculate `currentMaxPath` for `20`: `20 + 15 + 7 = 42`
       - Update `maxSum`: `max(15, 42) = 42`
       - Return: `20 + max(15, 7) = 35`
     - Calculate `currentMaxPath` for `-10`: `-10 + 9 + 35 = 34`
     - Update `maxSum`: `max(42, 34) = 42`
     - Return: `-10 + max(9, 35) = 25`
     
4. **Final Result**:
   - `maxSum = 42`

The maximum path sum is correctly found to be `42`, which corresponds to the path `15 -> 20 -> 7`.

This completes the dry run, illustrating how the solution works step by step.


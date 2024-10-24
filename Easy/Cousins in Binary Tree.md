### Code
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left),
 * right(right) {}
 * };
 */
class Solution {
public:
    bool isCousins(TreeNode* root, int x, int y) {
        if (root == nullptr)
            return false;
        if (x == y)
            return false;
        queue<TreeNode*> q;
        q.push(root);
        bool xFound = false, yFound = false;

        while (!q.empty()) {
            int s = q.size();

            // To simulate level order traversal,
            // only traverse the first s nodes in
            // the queue.
            while (s--) {
                TreeNode* curr = q.front();
                q.pop();

                if (curr->val == x)
                    xFound = true;
                if (curr->val == y) {
                    yFound = true;
                }

                // if x and y are children to the same node,
                // then return false.
                if (curr->left != nullptr && curr->right != nullptr &&
                    ((curr->left->val == x && curr->right->val == y) ||
                     (curr->left->val == y && curr->right->val == x)))
                    return false;

                // push left node into queue
                if (curr->left != nullptr)
                    q.push(curr->left);

                // Push right node into queue.
                if (curr->right != nullptr)
                    q.push(curr->right);
            }

            // After iteration of one level, following needs to
            // be checked

            // if both x and y are found,
            // return true.
            if (xFound && yFound)
                return true;

            // if one of x or y is found,
            // return false.
            if (xFound || yFound)
                return false;
        }

        return false;
    }
};
```

### Explanation
1. **Initial Checks**:  
   - If `root` is `nullptr`, return `false`.
   - If `x == y`, return `false` as cousins must be different nodes.

2. **Level-order Traversal**:  
   - A queue is used to perform level-order traversal (BFS) on the tree.
   - For each node at the current level:
     - If the node's value matches `x` or `y`, mark it as found.
     - Check if `x` and `y` are children of the same parent (if true, return `false`).
     - Push the node's children into the queue for further processing.

3. **Level End Conditions**:  
   - At the end of each level, check:
     - If both `x` and `y` were found at the same level, return `true`.
     - If only one of them was found, return `false`.

4. **Final Result**:  
   If the entire tree is traversed and neither condition for returning `true` or `false` is met, return `false`.

### Time Complexity
- **O(n)**, where `n` is the number of nodes in the tree. This is because we traverse every node once during the level-order traversal.

### Space Complexity
- **O(n)**, where `n` is the number of nodes in the tree. In the worst case, the queue can hold all nodes at the deepest level, which could be up to `n/2`.

### Dry Run
- **Input**: `root = [1,2,3,null,4,null,5], x = 5, y = 4`
- **Step 1**: Start from `root` (node 1), push it to the queue.
- **Step 2**: Process level 1: node 1 has children `2` and `3`. Add them to the queue.
- **Step 3**: Process level 2: nodes `2` and `3` are processed. `2` has a right child `4`, and `3` has a right child `5`. Add them to the queue.
- **Step 4**: Process level 3: nodes `4` and `5` are processed, both found at the same level but with different parents.
- **Output**: `true`, as `4` and `5` are cousins.

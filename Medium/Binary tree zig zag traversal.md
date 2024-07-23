# Method 1 (brute force)
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
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        if (root == nullptr) return {};
        vector<vector<int>> res;
        queue<TreeNode*> q;
        q.push(root);
        int flag = 0;
        while (!q.empty()) {
            int n = q.size();
            vector<int> temp;
            for (int i = 0; i < n; i++) {
                TreeNode* node = q.front();
                q.pop();
                temp.push_back(node->val);
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }
            if (flag) reverse(temp.begin(), temp.end());
            res.push_back(temp);
            flag = 1 - flag;
        }
        return res;
    }
};
```

### Explanation
The goal of this function is to traverse a binary tree in a zigzag level order and return the values in a `vector<vector<int>>`. In zigzag level order traversal, the nodes at each level are alternately traversed from left to right and right to left.

### Algorithm Steps:
1. **Initialization**:
   - If the root is `nullptr`, return an empty vector.
   - Initialize a result vector `res` to store the final output.
   - Use a queue `q` to facilitate level order traversal (BFS). Push the root node onto the queue.
   - Use a flag `flag` to toggle between normal and reversed order for each level.

2. **Level Order Traversal**:
   - While the queue is not empty, perform the following steps:
     - Determine the number of nodes at the current level (`n = q.size()`).
     - Initialize a temporary vector `temp` to store node values at the current level.
     - Iterate over all nodes at the current level:
       - Dequeue a node from the front of the queue.
       - Add the node's value to the `temp` vector.
       - Enqueue the node's left and right children if they exist.
     - If the flag is set, reverse the `temp` vector to achieve zigzag order.
     - Append `temp` to the result vector `res`.
     - Toggle the flag for the next level.

3. **Return the Result**:
   - After completing the traversal, return the result vector `res`.

### Time Complexity
- **Time Complexity**: \(O(N)\), where \(N\) is the number of nodes in the tree. Each node is visited exactly once.
- **Space Complexity**: \(O(N)\) due to the space used by the queue and the result vector.

### Dry Run Example
Consider the binary tree:

```
       1
     /   \
    2     3
   / \   / \
  4   5 6   7
```

1. **Initialization**:
   - `res = []`
   - `q = [1]`
   - `flag = 0`

2. **First Level**:
   - `n = 1`
   - `temp = []`
   - Process node 1:
     - `temp = [1]`
     - Enqueue 2 and 3
   - `flag` is 0, so no reversal
   - `res = [[1]]`
   - `flag = 1`

3. **Second Level**:
   - `n = 2`
   - `temp = []`
   - Process node 2:
     - `temp = [2]`
     - Enqueue 4 and 5
   - Process node 3:
     - `temp = [2, 3]`
     - Enqueue 6 and 7
   - `flag` is 1, so reverse `temp`
   - `res = [[1], [3, 2]]`
   - `flag = 0`

4. **Third Level**:
   - `n = 4`
   - `temp = []`
   - Process node 4:
     - `temp = [4]`
   - Process node 5:
     - `temp = [4, 5]`
   - Process node 6:
     - `temp = [4, 5, 6]`
   - Process node 7:
     - `temp = [4, 5, 6, 7]`
   - `flag` is 0, so no reversal
   - `res = [[1], [3, 2], [4, 5, 6, 7]]`
   - `flag = 1`

5. **Completion**:
   - Queue is empty, return `res`.

The final result is `[[1], [3, 2], [4, 5, 6, 7]]`.

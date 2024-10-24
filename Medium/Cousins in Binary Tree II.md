### Code
```cpp
class Solution {
public:
    vector<int> depthSum;
    // to calculate the sum at each depth (with curr depth: d) and store in the vector
    void dfs1(TreeNode* root, int d) {
        if (!root) return;
        if (d>=depthSum.size()) depthSum.push_back(root->val);
        else depthSum[d]+=root->val;
        dfs1(root->left, d+1);
        dfs1(root->right, d+1);
    }

    // to replace each node's value with the sum of its cousins' values
    void dfs2(TreeNode* root, int val, int d) {
        root->val=val;
        int c=0;
        if (d+1<depthSum.size()) {
            c=depthSum[d+1];
        }
        if (root->left!=nullptr) {
            c-=root->left->val;
        }
        if (root->right!=nullptr) {
            c-=root->right->val;
        }
        if (root->left) dfs2(root->left, c, d+1);
        if (root->right) dfs2(root->right, c, d+1);
    }

    TreeNode* replaceValueInTree(TreeNode* root) {
        dfs1(root, 0);
        dfs2(root, 0, 0);
        return root;
    }
};

```

### **Problem Description**
We are asked to replace each node's value with the sum of its cousins' values. Cousins are nodes that are at the same depth as the current node but have different parents.

### **Code Overview**

The solution uses two depth-first search (DFS) traversals:
1. **First DFS (`dfs1`)**: This traversal calculates the sum of all node values at each depth level.
2. **Second DFS (`dfs2`)**: This traversal modifies each node's value to be the sum of its cousins.

### **Detailed Explanation**

1. **First DFS (`dfs1`)**: 
   - This function traverses the tree and calculates the sum of the node values at each depth level. 
   - It uses a vector `depthSum` where `depthSum[i]` stores the sum of all node values at depth `i`.
   - If the current depth `d` is greater than or equal to the size of the vector, it adds a new element to the vector with the current node's value. If the depth already exists, it adds the current node's value to the existing sum.

   **Purpose**: After this DFS, we will have the sum of node values for each depth level.

   Example:
   ```
   If the tree is:
        1
       / \
      2   3
     / \   \
    4   5   6
   
   depthSum will be: [1, 5, 15]  // sum of nodes at depth 0, 1, and 2
   ```

2. **Second DFS (`dfs2`)**: 
   - The goal here is to replace each node's value with the sum of its cousins' values.
   - For each node, the sum of cousin nodes is calculated by:
     - Starting with the sum of all node values at the next depth (`depthSum[d + 1]`).
     - Subtracting the values of the current node's children (if they exist), because the children are not cousins.
   - After calculating this cousin sum, the function recursively applies the same process to the left and right children of the node.

   **Cousin Sum Calculation**: 
   - The cousin sum for a node at depth `d` is computed as the sum of all nodes at depth `d + 1` minus the values of its own children (since cousins do not include its own children).
   
   Example:
   ```
   For the node 2 in the example tree:
   - The sum of nodes at depth 2 is 15 (nodes 4, 5, and 6).
   - Node 2's children are 4 and 5.
   - So, the cousin sum for node 2 is 15 - 4 - 5 = 6 (node 6 is the cousin).
   
   For the node 3:
   - The sum of nodes at depth 2 is still 15.
   - Node 3's child is 6.
   - So, the cousin sum for node 3 is 15 - 6 = 9 (nodes 4 and 5 are cousins).
   ```

3. **Main Function (`replaceValueInTree`)**:
   - This function simply calls `dfs1` to calculate the depth sums and then calls `dfs2` to replace the node values with the sum of their cousins.

### **Example of Cousin Sum Calculation**

Consider this tree:
```
       1
      / \
     2   3
    / \   \
   4   5   6
```

- After running `dfs1`, we get the `depthSum`:
  - Depth 0: 1
  - Depth 1: 5 (2 + 3)
  - Depth 2: 15 (4 + 5 + 6)

- Now, `dfs2` modifies each node's value:
  - The root (1) is replaced with `0` (as there are no cousins at depth 0).
  - Node 2 is replaced with `6` (the cousin sum at depth 2 is 6, which is node 6's value).
  - Node 3 is replaced with `9` (the cousin sum at depth 2 is 9, which is the sum of nodes 4 and 5).
  - Nodes 4, 5, and 6 are leaf nodes and don’t have cousins, so their values become `0`.

### **Complexity Analysis**

- **Time Complexity**: 
  - The first DFS (`dfs1`) traverses each node exactly once to compute the depth sums. This takes `O(n)`, where `n` is the number of nodes in the tree.
  - The second DFS (`dfs2`) also traverses each node exactly once to replace the values. This also takes `O(n)`.
  - Hence, the total time complexity is `O(n)`.

- **Space Complexity**: 
  - The main extra space used is the `depthSum` vector, which stores one value for each depth. In the worst case, this takes `O(h)` space, where `h` is the height of the tree.
  - The recursive stack for DFS will also use `O(h)` space.
  - Thus, the total space complexity is `O(h)`.

This approach efficiently calculates and replaces each node's value with the sum of its cousins' values.

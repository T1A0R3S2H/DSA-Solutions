# Method 1 (brute-force), O(n^2)
```cpp
class Solution {
private:
    int height(TreeNode *node){
        if (node==nullptr) return 0;
        int left=height(node->left);
        int right=height(node->right);
        return max(left, right)+1;
    }
    
public:
    bool isBalanced(TreeNode *root)
    {
        if (root==nullptr) return true;
        bool left=isBalanced(root->left);
        bool right=isBalanced(root->right);
        bool diff=abs(height(root->left)-height(root->right))<=1;
        return left && right && diff;
    }
};
```

# Method 2 (single traversal), O(n), O(h)
```cpp
class Solution {
public:
    pair<int, bool> check(TreeNode* node) {
        if (node == nullptr) return {0, true};
        auto left = check(node->left);
        auto right = check(node->right);
        int height = max(left.first, right.first) + 1;
        bool isBalanced = left.second && right.second && abs(left.first - right.first) <= 1;
        return {height, isBalanced};
    }
    
    bool isBalanced(TreeNode *root) {
        return check(root).second;
    }
};
```
Certainly! Let's break down the optimized approach step by step.

### Objective
To determine if a binary tree is height-balanced. A binary tree is balanced if, for every node, the height difference between its left and right subtrees is at most 1.

### Key Idea
The key idea is to combine the calculation of the height of the tree and the check for balance into a single traversal. This avoids redundant computations, ensuring that each node is processed only once.

### Implementation

1. **Helper Function: `checkHeightAndBalance`**
   This function returns a pair consisting of:
   - The height of the subtree.
   - A boolean indicating whether the subtree is balanced.

   ```cpp
   pair<int, bool> checkHeightAndBalance(TreeNode* node) {
       if (node == nullptr) return {0, true};
       
       auto left = checkHeightAndBalance(node->left);
       auto right = checkHeightAndBalance(node->right);
       
       int height = max(left.first, right.first) + 1;
       bool isBalanced = left.second && right.second && abs(left.first - right.first) <= 1;
       
       return {height, isBalanced};
   }
   ```

   - **Base Case**: If the node is `nullptr`, the height is `0` and it is considered balanced (`true`).
   - **Recursive Case**:
     - Recursively compute the height and balance status of the left and right subtrees.
     - Calculate the current node's height as `max(left height, right height) + 1`.
     - Determine if the current node is balanced by ensuring:
       - Both left and right subtrees are balanced.
       - The height difference between the left and right subtrees is at most 1.

2. **Main Function: `isBalanced`**
   This function starts the recursive process and returns the balance status of the tree.

   ```cpp
   bool isBalanced(TreeNode *root) {
       return checkHeightAndBalance(root).second;
   }
   ```

### How It Works

1. **Recursive Traversal**:
   - Starting from the root, the function `checkHeightAndBalance` recursively visits every node in the tree.
   - For each node, it computes the height of its left and right subtrees and checks if they are balanced.

2. **Height Calculation**:
   - The height of each node is computed as `max(height of left subtree, height of right subtree) + 1`.

3. **Balance Check**:
   - For each node, the function checks if the left and right subtrees are balanced and if the height difference between them is at most 1.

4. **Combining Results**:
   - The function returns a pair containing the height and balance status of each subtree.
   - If both subtrees are balanced and their height difference is within the allowed range, the current subtree is balanced.

### Example

Consider the following tree:
```
      1
     / \
    2   3
   / \
  4   5
```

1. **Node 4 and 5**:
   - Both are leaf nodes, so their height is `1` and they are balanced.

2. **Node 2**:
   - Height of left subtree (node 4) = 1
   - Height of right subtree (node 5) = 1
   - Height of node 2 = `max(1, 1) + 1 = 2`
   - Node 2 is balanced since `|1 - 1| <= 1` and both subtrees are balanced.

3. **Node 3**:
   - It is a leaf node, so its height is `1` and it is balanced.

4. **Root Node (1)**:
   - Height of left subtree (node 2) = 2
   - Height of right subtree (node 3) = 1
   - Height of root = `max(2, 1) + 1 = 3`
   - The tree is balanced since `|2 - 1| <= 1` and both subtrees are balanced.

### Efficiency

- **Time Complexity**: \(O(n)\)
  - Each node is visited exactly once, so the time complexity is linear with respect to the number of nodes.

- **Space Complexity**: \(O(h)\)
  - The space complexity is determined by the maximum depth of the recursion stack, which is the height of the tree. For a balanced tree, this is \(O(\log n)\). For an unbalanced tree, it can be \(O(n)\).

By using this combined approach, we ensure that the solution is both time and space efficient, achieving the desired result with minimal overhead.

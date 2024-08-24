### Code
```cpp
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (!root) return NULL;
        invertTree(root->left); //left subtree
        invertTree(root->right); //right subtree
        
        // Swap the left and right children of the current node.
        TreeNode* temp=root->left;
        root->left=root->right;
        root->right=temp;
        return root;
    }
};
```
### Explanation

The goal of the `invertTree` function is to invert a binary tree, meaning the left and right children of all nodes are swapped. This can be done recursively by performing the following steps:

1. **Base Case:** If the current node (`root`) is `NULL`, return `NULL`. This is the stopping condition for the recursion, which occurs when the function reaches the leaves of the tree.

2. **Recursive Case:** If the current node is not `NULL`, recursively invert the left and right subtrees. This is done by calling `invertTree(root->left)` and `invertTree(root->right)`.

3. **Swap Children:** After inverting the left and right subtrees, swap the left and right children of the current node. This effectively inverts the tree at the current node level.

4. **Return the Root:** Finally, return the root of the inverted tree. This allows the changes to propagate up the recursion stack.

### Time Complexity Analysis

The time complexity of the `invertTree` function is **O(n)**, where `n` is the number of nodes in the binary tree. This is because each node is visited exactly once to perform the swap operation.

### Space Complexity Analysis

The space complexity of the function is **O(h)**, where `h` is the height of the binary tree. This space is used by the recursion stack. In the worst case, for a skewed tree, `h` could be equal to `n` (making the space complexity O(n)), while in the best case (balanced tree), `h` would be log(n).

### Dry Run (ETSD)

Let's dry-run the function with a simple binary tree as an example:

**Original Tree:**

```
      1
     / \
    2   3
   /   / \
  4   5   6
```

**Step-by-Step Execution:**

1. **Call `invertTree(1)`**  
   - `root` is not `NULL`.
   - Recursively call `invertTree(2)` for the left subtree.

2. **Call `invertTree(2)`**  
   - `root` is not `NULL`.
   - Recursively call `invertTree(4)` for the left subtree.

3. **Call `invertTree(4)`**  
   - `root` is not `NULL`.
   - Recursively call `invertTree(NULL)` for both left and right subtrees (since `4` is a leaf).
   - Return `4` as it remains the same.

4. **Back to `invertTree(2)`**  
   - Left subtree (4) is inverted (no change for leaf).
   - Recursively call `invertTree(NULL)` for the right subtree (since `2` has no right child).
   - Swap left and right children of `2`.
   - `2` now has:
     ```
       2
        \
         4
     ```
   - Return `2`.

5. **Back to `invertTree(1)`**  
   - Left subtree (2) is now:
     ```
       2
        \
         4
     ```
   - Recursively call `invertTree(3)` for the right subtree.

6. **Call `invertTree(3)`**  
   - `root` is not `NULL`.
   - Recursively call `invertTree(5)` for the left subtree.

7. **Call `invertTree(5)`**  
   - `root` is not `NULL`.
   - Recursively call `invertTree(NULL)` for both left and right subtrees (since `5` is a leaf).
   - Return `5` as it remains the same.

8. **Back to `invertTree(3)`**  
   - Left subtree (5) is inverted (no change for leaf).
   - Recursively call `invertTree(6)` for the right subtree.

9. **Call `invertTree(6)`**  
   - `root` is not `NULL`.
   - Recursively call `invertTree(NULL)` for both left and right subtrees (since `6` is a leaf).
   - Return `6` as it remains the same.

10. **Back to `invertTree(3)`**  
    - Right subtree (6) is inverted (no change for leaf).
    - Swap left and right children of `3`.
    - `3` now has:
      ```
        3
       / \
      6   5
      ```
    - Return `3`.

11. **Back to `invertTree(1)`**  
    - Right subtree (3) is now:
      ```
        3
       / \
      6   5
      ```
    - Swap left and right children of `1`.
    - The tree is now:
      ```
        1
       / \
      3   2
     / \   \
    6   5   4
      ```
    - Return `1`.

**Final Inverted Tree:**

```
      1
     / \
    3   2
   / \   \
  6   5   4
```

The tree has been successfully inverted, and the function returns the root of the inverted tree.

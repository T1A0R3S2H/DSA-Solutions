
### Explanation
The `deleteNode` function deletes a node with a specific key in a binary search tree (BST). Step-by-step explanation:

1. **Base Case**:
    - If the `root` is `NULL`, it means the tree is empty or we've reached a leaf node's child. So, return `NULL`.

2. **Key Found**:
    - If the `root`'s value matches the `key`, there are three cases to consider:
        - **No Right Child**: 
            - Return the left child, effectively bypassing the root.
        - **No Left Child**: 
            - Return the right child, effectively bypassing the root.
        - **Two Children**: 
            - Find the smallest value in the right subtree (or the largest in the left subtree), swap it with the root's value, and delete the smallest value from the right subtree.

3. **Key Not Found**:
    - Recursively call `deleteNode` on the left and right subtrees to continue searching for the key.

### Time Complexity (TC)
- In the worst case, the function traverses from the root to a leaf, which in a skewed tree (like a linked list) can be \(O(n)\), where \(n\) is the number of nodes in the tree.
- For a balanced BST, the average case time complexity is \(O(\log n)\).

### Space Complexity (SC)
- The space complexity is determined by the recursion stack. In the worst case, it can be \(O(n)\) for a skewed tree. For a balanced tree, the space complexity is \(O(\log n)\).

### Dry Run
Let's perform a dry run with a sample BST and key.

**Example Tree**:
```
       5
      / \
     3   7
    / \   \
   2   4   8
```
**Key**: 3

**Steps**:
1. `root = 5, key = 3`
    - `root->val` is not equal to `key`. Call `deleteNode` on `root->left`.
    
2. `root = 3, key = 3`
    - `root->val` equals `key`. Node 3 has two children.
    - Find the smallest value in the right subtree of node 3, which is node 4.
    - Swap values of nodes 3 and 4.
    - Delete node 4.
    
3. `root = 4, key = 4`
    - `root->val` equals `key`. Node 4 has no children.
    - Return `NULL`, effectively deleting node 4.

**Final Tree**:
```
       5
      / \
     4   7
    /     \
   2       8
```

### Code with Comments
```cpp
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        ios_base::sync_with_stdio(false);
        cin.tie(nullptr);
        cout.tie(nullptr);

        // Base case: if root is NULL, return NULL
        if (!root)
            return NULL;

        // If the root's value matches the key
        if (root->val == key) {
            // No right child case
            if (!root->right) {
                TreeNode* left = root->left;
                delete root;
                return left;
            }
            // No left child case
            else if (!root->left) {
                TreeNode* right = root->right;
                delete root;
                return right;
            }
            // Both children exist
            else {
                TreeNode* right = root->right;
                // Find the smallest value in the right subtree
                while (right->left) {
                    right = right->left;
                }
                // Swap the values
                swap(root->val, right->val);
            }
        }

        // Recur for left and right subtrees
        root->left = deleteNode(root->left, key);
        root->right = deleteNode(root->right, key);
        return root;
    }
};
```

This should give a clear understanding of how the `deleteNode` function works along with its time and space complexities.

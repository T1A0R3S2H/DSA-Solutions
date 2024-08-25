### Code
```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root==NULL) return root;
        if(root->val==p->val || root->val==q->val) return root;
        TreeNode* left=lowestCommonAncestor(root->left, p, q);
        TreeNode* right=lowestCommonAncestor(root->right, p, q);
        if(left!=NULL && right!=NULL) return root;
        else if(left!=NULL && right==NULL) return left;
        else return right;
    }
};
```
### Explanation

1. **Base Case**:
    - If the `root` is `NULL`, the function returns `NULL`. This indicates that we've reached the end of a branch without finding either `p` or `q`.
    - If the `root` node itself is either `p` or `q`, it returns `root`. This means one of the nodes is found, and it might be the LCA.

2. **Recursive Case**:
    - The function calls itself recursively for the left and right subtrees.
    - `TreeNode* left = lowestCommonAncestor(root->left, p, q);`: This searches for `p` and `q` in the left subtree.
    - `TreeNode* right = lowestCommonAncestor(root->right, p, q);`: This searches for `p` and `q` in the right subtree.
   
3. **Decision Making**:
    - If both `left` and `right` are non-NULL, this means that `p` is found in one subtree and `q` is found in the other. Hence, the current node `root` is the LCA, and it returns `root`.
    - If only `left` is non-NULL, it means both `p` and `q` were found in the left subtree, and it returns `left` as the LCA.
    - If only `right` is non-NULL, it means both `p` and `q` were found in the right subtree, and it returns `right` as the LCA.

### Time Complexity Analysis

- Each node in the binary tree is visited once, so the time complexity is **O(n)**, where `n` is the number of nodes in the tree.

### Space Complexity Analysis

- The space complexity is determined by the recursion stack, which can go as deep as the height of the tree. In the worst case (unbalanced tree), the space complexity is **O(n)**. In the best case (balanced tree), it is **O(h)**, where `h` is the height of the tree.

### Dry Run

Consider the following binary tree and nodes `p = 5` and `q = 1`:

```
        3
       / \
      5   1
     / \   \
    6   2   8
       / \
      7   4
```

- `lowestCommonAncestor(root=3, p=5, q=1)`
  - Root is not `NULL` and not equal to `p` or `q`.
  - Recurse on the left subtree: `lowestCommonAncestor(root=5, p=5, q=1)`
    - Root is equal to `p`, return `5`.
  - Recurse on the right subtree: `lowestCommonAncestor(root=1, p=5, q=1)`
    - Root is equal to `q`, return `1`.
  - Both left and right results are non-NULL (`5` and `1`), return `3`.

Result: The LCA of nodes `5` and `1` is `3`.

This approach effectively finds the LCA by checking each node's subtree for the presence of `p` and `q` and determining the lowest common point where both nodes are found.

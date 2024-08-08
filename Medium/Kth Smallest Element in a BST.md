# Method 1 (inorder traversal)
When you perform an in-order traversal on a binary search tree (BST), you visit the nodes in ascending order of their values. So, for your first example:

Given the tree:

```
    3
   / \
  1   4
   \
    2
```

An in-order traversal would visit the nodes in the following order:

1. Visit the left subtree: 
   - Visit node 1 (left of 3)
   - Visit node 2 (right child of 1)
   
2. Visit the root node 3.
3. Visit the right subtree:
   - Visit node 4 (right of 3)

Thus, the in-order traversal for this tree results in the list `[1, 2, 3, 4]`.

- For `k = 1`, the 1st smallest element is `1`.
- For `k = 2`, the 2nd smallest element is `2`.
- For `k = 3`, the 3rd smallest element is `3`.
- For `k = 4`, the 4th smallest element is `4`.

So, the `kthSmallest` function correctly returns the k-th smallest value based on this in-order traversal list.
- ie. we will return ans[k-1] as our ans.

### Code

```cpp
class Solution {
public:
    int kthSmallest(TreeNode* root, int k) {
        vector<int> ans;
        inorder(root, ans);
        int n=ans.size();
        return ans[k-1];
    }

    void inorder(TreeNode* root, vector<int>& ans) {
        if (root==nullptr) return;
        inorder(root->left, ans);
        ans.push_back(root->val);
        inorder(root->right, ans);
    }
};
```

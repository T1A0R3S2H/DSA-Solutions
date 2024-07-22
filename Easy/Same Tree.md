# Method 1

```cpp
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (p == nullptr && q == nullptr)
            return true;
        if (p == nullptr || q == nullptr)
            return false;
        if (p != NULL && q != NULL) {
            return (p->val == q->val) && isSameTree(p->left, q->left) &&
                   isSameTree(p->right, q->right);
        }
        return false;
    }
};
```
The approach used in the `isSameTree` function is a recursive one to determine if two binary trees are structurally identical and have the same node values. Here's a step-by-step explanation:

1. **Base Case - Both Nodes are Null:**
   ```cpp
   if (p == nullptr && q == nullptr)
       return true;
   ```
   - If both `p` and `q` are `nullptr`, it means we've reached the end of both trees simultaneously, and they are identical up to this point, so return `true`.

2. **Base Case - One Node is Null:**
   ```cpp
   if (p == nullptr || q == nullptr)
       return false;
   ```
   - If one of the nodes is `nullptr` and the other is not, it means the trees are not structurally identical, so return `false`.

3. **Recursive Case - Both Nodes are Non-Null:**
   ```cpp
   if (p != nullptr && q != nullptr) {
       return (p->val == q->val) && isSameTree(p->left, q->left) &&
              isSameTree(p->right, q->right);
   }
   ```
   - If both `p` and `q` are non-null, first check if the values of the current nodes are the same (`p->val == q->val`).
   - Then, recursively check if the left subtrees of `p` and `q` are the same (`isSameTree(p->left, q->left)`).
   - Also, recursively check if the right subtrees of `p` and `q` are the same (`isSameTree(p->right, q->right)`).
   - The `return` statement ensures that all these conditions must be true for the trees to be considered the same.

4. **Ensure All Code Paths Return a Value:**
   ```cpp
   return false;
   ```
   - This line is necessary to handle any other paths that might occur, ensuring the function always returns a boolean value. Although, in the current logic flow, this line will never be reached because all possible cases are already covered by the previous conditions.

### Example

Consider two trees `p` and `q`:

```
Tree p:         Tree q:
    1               1
   / \             / \
  2   3           2   3
```

- The function starts with the root nodes of both trees.
- It checks if `p` and `q` are both null (they are not).
- It checks if one is null (they are not).
- It checks if `p->val == q->val` (1 == 1, which is true).
- It then recursively checks the left subtrees (`isSameTree(p->left, q->left)`), which are both `2`.
  - The process repeats: both nodes are non-null, values are equal, so it checks their left and right children, which are null, resulting in true.
- It recursively checks the right subtrees (`isSameTree(p->right, q->right)`), which are both `3`.
  - Similarly, both nodes are non-null, values are equal, and their children are null, resulting in true.
- Since all conditions are satisfied, the function returns `true`, indicating that trees `p` and `q` are the same.

This recursive approach ensures that each node and its children are checked for equality, providing a comprehensive comparison of the two trees.

To determine the time complexity (TC) and space complexity (SC) of the `isSameTree` function, let's analyze the function in detail:

### Time Complexity (TC)

The time complexity of the function is determined by how many times the function is called and how much work is done in each call.

1. **Function Calls**:
   - The function is called once for each node in the trees. In the worst case, every node in both trees will be visited.

2. **Work Per Call**:
   - Each call to `isSameTree` does a constant amount of work: it checks if the nodes are `nullptr`, compares their values, and makes recursive calls.

If the trees each have `n` nodes, the function will visit each node once. Therefore, the time complexity is \( O(n) \).

### Space Complexity (SC)

The space complexity is determined by the maximum depth of the recursion stack, which corresponds to the height of the tree.

1. **Recursion Depth**:
   - In the worst case, the tree is completely unbalanced, and the recursion depth is equal to the number of nodes, which is `n`.
   - In the best case, the tree is balanced, and the recursion depth is the height of the tree, which is \( O(\log n) \) for a balanced binary tree.

2. **Auxiliary Space**:
   - Apart from the recursion stack, the function does not use any additional space that scales with the input size.

So, the space complexity is:
- \( O(n) \) in the worst case (completely unbalanced tree).
- \( O(\log n) \) in the best case (completely balanced tree).

### Summary

- **Time Complexity**: \( O(n) \), where `n` is the number of nodes in the tree.
- **Space Complexity**: \( O(n) \) in the worst case, \( O(\log n) \) in the best case.

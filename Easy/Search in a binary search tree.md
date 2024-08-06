```cpp
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        if (root == nullptr) return nullptr;
        if (root->val == val) return root;
        if (root->val > val) {
            return searchBST(root->left, val);
        } 
        else {
            return searchBST(root->right, val);
        }
    }
};
```
### Explanation

The function `searchBST` takes two parameters: a pointer to the root node of the BST (`root`) and the value to search for (`val`). 

1. **Base Case**:
   - If `root` is `nullptr`, it means we've reached the end of a branch and the value was not found in the tree. Thus, we return `nullptr`.

2. **Value Found**:
   - If the current node's value (`root->val`) is equal to `val`, we return the current node (`root`).

3. **Value Less Than Node's Value**:
   - If `val` is less than `root->val`, we continue the search in the left subtree by recursively calling `searchBST` with `root->left`.

4. **Value Greater Than Node's Value**:
   - If `val` is greater than `root->val`, we continue the search in the right subtree by recursively calling `searchBST` with `root->right`.

### Time Complexity (TC)

- **Best Case**: \( O(1) \)
  - If the root node itself contains the value `val`, the function will return immediately.

- **Average and Worst Case**: \( O(h) \)
  - The time complexity is proportional to the height of the tree (`h`). In the worst case, the tree might be skewed (like a linked list), making the height of the tree equal to the number of nodes (`n`). Thus, in the worst case, the time complexity would be \( O(n) \).
  - In a balanced BST, the height is \( O(\log n) \), where `n` is the number of nodes.

### Space Complexity (SC)

- **Best Case**: \( O(1) \)
  - If the root node itself contains the value `val`, no additional space is used.

- **Average and Worst Case**: \( O(h) \)
  - The space complexity is determined by the maximum depth of the recursive call stack, which is equal to the height of the tree (`h`). In the worst case of a skewed tree, the space complexity would be \( O(n) \).
  - In a balanced BST, the space complexity is \( O(\log n) \) due to the recursive call stack depth.

### Example

```cpp
// Given BST
//         4
//        / \
//       2   7
//      / \
//     1   3

Solution solution;
TreeNode* result = solution.searchBST(root, 2);
// result now points to the node with value 2, which has the subtree:
//       2
//      / \
//     1   3
```

This code efficiently searches for a value in a BST by leveraging the BST properties, ensuring that the search is conducted in logarithmic time in the best scenarios (balanced trees).

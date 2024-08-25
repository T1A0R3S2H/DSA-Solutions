The 'Ceil in BST' problem is similar to the 'Floor in BST' problem but with the opposite goal: finding the smallest value node in the BST that is greater than or equal to a given value `x`. If no such node exists (i.e., `x` is greater than the largest node in the BST), the function should return `-1`.

### Explanation:
To find the ceil value in a BST, we can leverage the properties of the BST:
1. If a node’s value is greater than `x`, then it could be a potential ceil value. We move to the left subtree to check if there’s a smaller value that still satisfies the condition.
2. If a node’s value is less than `x`, then it cannot be a ceil value, so we move to the right subtree.
3. If we find a node with a value exactly equal to `x`, this is the ceil value, and we return it immediately.

### Approach:
1. Start from the root node.
2. Initialize a variable `ceilValue` to `-1` to store the potential ceil value.
3. Traverse the tree:
   - If the current node’s value is equal to `x`, return it as the ceil value.
   - If the current node’s value is greater than `x`, update `ceilValue` to the current node’s value and move to the left child to find a smaller potential value.
   - If the current node’s value is less than `x`, move to the right child.
4. If no exact match is found, return the `ceilValue`.

### Implementation in C++:

```cpp
int findCeil(Node* root, int input) {
    int ceilValue = -1;
    while (root) {
        if (root->data == input) {
            return root->data; // Exact match, return as ceil.
        } else if (root->data > input) {
            ceilValue = root->data; // Update ceilValue, go to left subtree.
            root = root->left;
        } else {
            root = root->right; // Go to right subtree to find a greater value.
        }
    }
    return ceilValue; // Return the last recorded ceil value.
}
```

### Explanation of the Code:
1. The function starts by initializing `ceilValue` to `-1`, which serves as a placeholder.
2. The `while` loop iterates through the tree nodes:
   - If the current node’s value equals `input`, we immediately return this value.
   - If the current node’s value is greater than `input`, this value is a potential ceil value, so we update `ceilValue` and move to the left child to find a closer match.
   - If the current node’s value is less than `input`, we move to the right child because we need a larger value.

### Time Complexity Analysis:
- **Time Complexity**: \( O(\text{height of the tree}) \). In the worst-case scenario (for a skewed tree), this can be \( O(n) \), where `n` is the number of nodes. For a balanced BST, the height is \( O(\log n) \), making the time complexity \( O(\log n) \).

### Space Complexity Analysis:
- **Space Complexity**: \( O(1) \) because the algorithm uses only a few extra variables, regardless of the size of the input tree. It does not use any auxiliary data structures that grow with the input size.

### Dry Run:

Let's do a dry run of the algorithm with an example:

**Example Tree:**

```
       2
        \
        81
       /   \
     42    87
       \     \
       66    90
      /
    45
```

**Input:** `input = 43`

1. Initialize `ceilValue = -1`.
2. Start at the root node: `2`.
   - `2 < 43` → Move to the right child.
3. Current node: `81`.
   - `81 > 43` → Update `ceilValue = 81`. Move to the left child.
4. Current node: `42`.
   - `42 < 43` → Move to the right child.
5. Current node: `66`.
   - `66 > 43` → Update `ceilValue = 66`. Move to the left child.
6. Current node: `45`.
   - `45 > 43` → Update `ceilValue = 45`. Move to the left child, which is `NULL`.

**Output:** `45` (correct, as 45 is the smallest node greater than or equal to 43).

This dry run shows how the algorithm efficiently finds the smallest value greater than or equal to `x` by utilizing the BST properties.

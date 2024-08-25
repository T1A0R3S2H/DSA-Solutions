### Code
```cpp
class Solution {
public:
    Node* connect(Node* root) {
        if (!root) return nullptr; // Explicitly return nullptr if the root is null
        Node* leftmost = root;
        while (leftmost->left) {
            Node* current = leftmost;
            while (current) {
                current->left->next = current->right; // connect left child to right child
                if (current->next) { 
                    current->right->next = current->next->left; // connect right child to next left child
                }
                current = current->next; // move to the next node in the current level
            }
            leftmost = leftmost->left; // move to the next level
        }
        return root; // Return the modified tree's root
    }
};
```

### Explanation

1. **Initialization**:
   - The function starts by checking if the `root` is `nullptr`. If it is, the function returns `nullptr` because there's nothing to connect.
   - The `leftmost` pointer is initialized to the root. This pointer helps to traverse the leftmost node of each level.

2. **Outer Loop** (`while (leftmost->left)`):
   - This loop iterates over each level of the tree. Since it's a perfect binary tree, if a node has a left child, it implies it also has a right child, and there are more levels to process.
   - We exit the loop when we reach the leaf level (the last level), where the nodes do not have any children.

3. **Inner Loop** (`while (current)`):
   - `current` is initialized to `leftmost` to traverse the nodes at the current level.
   - For each node:
     - The left child's `next` pointer is set to the right child (`current->left->next = current->right`).
     - If the `current` node has a `next` pointer (meaning there are more nodes at the same level), the right child's `next` pointer is set to the left child of the `current` node's next neighbor (`current->right->next = current->next->left`).
   - `current` is moved to the next node in the current level (`current = current->next`).

4. **Move Down a Level**:
   - After processing all nodes at the current level, `leftmost` is moved to its left child (`leftmost = leftmost->left`). This moves the traversal to the next level.

5. **Return the Root**:
   - The function returns the modified root, completing the connection of all `next` pointers.

### Time Complexity

- **Time Complexity**: O(N), where N is the number of nodes in the tree. Each node is visited once to set its `next` pointer.

### Space Complexity

- **Space Complexity**: O(1), since the algorithm uses only a fixed number of pointers (`leftmost` and `current`) for traversal and does not use any additional space that scales with the input size. 

### Dry Run

Let's perform a dry run using the input tree `[1, 2, 3, 4, 5, 6, 7]`:

```
        1
      /   \
     2     3
    / \   / \
   4   5 6   7
```

#### Initial State

- `leftmost` points to the root node (1).

#### First Level

- `current = leftmost = 1`
    - Connect `1->left` (2) to `1->right` (3): `2->next = 3`
    - No `1->next` exists, so nothing more to connect at this level.
- Move `leftmost` to `leftmost->left` (2).

#### Second Level

- `leftmost = 2`
- `current = leftmost = 2`
    - Connect `2->left` (4) to `2->right` (5): `4->next = 5`
    - `2->next` exists, connect `2->right` (5) to `2->next->left` (6): `5->next = 6`
    - Move `current` to `2->next` (3)
        - Connect `3->left` (6) to `3->right` (7): `6->next = 7`
        - No `3->next` exists, so nothing more to connect at this level.
- Move `leftmost` to `leftmost->left` (4).

#### Third Level

- `leftmost = 4`
- At this level, `leftmost->left` is `nullptr`, indicating all levels are processed. Exit the loop.

### Final Tree with `next` Pointers

The tree now has `next` pointers as follows:

```
        1 -> NULL
      /   \
     2  -> 3 -> NULL
    / \   / \
   4->5->6->7 -> NULL
```

The `next` pointers correctly connect each node to its adjacent right node, with the last node of each level pointing to `NULL`. The output would be `[1,#,2,3,#,4,5,6,7,#]`, where `#` denotes the end of each level.

This approach efficiently sets up the `next` pointers using constant extra space, meeting the problem's constraints.

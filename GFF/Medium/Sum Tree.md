# Using BFS (level order traversal) and recurasion calls for sum at each level, TC:(O(n^2), SC: O(n)
```cpp
#include <queue>

class Solution
{
public:
    bool isSumTree(Node* root)
    {
        if (!root) return true; // An empty tree is a Sum Tree
        return isSumTreeUtil(root);
    }

private:
    // Function to check if a tree is a sum tree
    bool isSumTreeUtil(Node* root)
    {
        if (!root) return true;
        if (!root->left && !root->right) return true; // Leaf nodes are sum trees
        
        int leftSum = sum(root->left);
        int rightSum = sum(root->right);
        
        if (root->data == leftSum + rightSum &&
            isSumTreeUtil(root->left) &&
            isSumTreeUtil(root->right))
        {
            return true;
        }
        
        return false;
    }

    // Function to calculate the sum of nodes in a tree
    int sum(Node *node)
    {
        if (!node) return 0;
        
        std::queue<Node*> q;
        q.push(node);
        int totalSum = 0;
        
        while (!q.empty())
        {
            Node *current = q.front();
            q.pop();
            
            totalSum += current->data;
            
            if (current->left) q.push(current->left);
            if (current->right) q.push(current->right);
        }
        
        return totalSum;
    }
};

```
### Approach:

1. **Level Order Traversal for Sum Calculation:**
   - The `sum` function calculates the sum of all nodes in a given subtree using level order traversal (BFS). It uses a queue to traverse each node in the subtree, adding each node's value to the total sum.
   - This traversal ensures that each node is visited exactly once, making the sum calculation straightforward and efficient.

2. **Sum Tree Validation:**
   - The `isSumTreeUtil` function checks if the current node is a sum tree.
   - It calculates the sum of the left and right subtrees using the `sum` function.
   - It compares the current node's value to the sum of its left and right subtrees.
   - It recursively checks if both left and right subtrees are also sum trees.

3. **Recursive Checks:**
   - For each node, after ensuring that the node's value equals the sum of its left and right subtrees, it recursively verifies that the left and right children are also sum trees.

### Time Complexity (TC):

- **`sum` Function:**
  - Each node is visited once in the level order traversal.
  - If there are `N` nodes in the subtree, the time complexity to compute the sum is `O(N)`.

- **`isSumTreeUtil` Function:**
  - For each node, the `sum` function is called, which has a time complexity of `O(N)`.
  - Additionally, `isSumTreeUtil` is called recursively on both left and right subtrees.
  - Thus, for the entire tree, the overall time complexity is approximately `O(N^2)` in the worst case, because the `sum` function is called multiple times.

### Space Complexity (SC):

- **Queue in `sum` Function:**
  - The queue used for BFS can store up to `N` nodes at its maximum, which is the size of the subtree.
  - Space complexity due to the queue is `O(N)`.

- **Recursive Call Stack in `isSumTreeUtil` Function:**
  - The maximum depth of the recursion is the height of the tree, which is `O(H)`, where `H` is the height of the tree.
  - In a balanced binary tree, the height `H` is `O(log N)`, but in the worst case (completely unbalanced tree), it can be `O(N)`.

- **Overall Space Complexity:**
  - Combining both the space for the queue and the recursion stack, the space complexity is `O(N)`.

### Summary:

- **Time Complexity:** `O(N^2)` in the worst case due to repeated calls to the `sum` function.
- **Space Complexity:** `O(N)` due to the space required for the queue and recursive call stack.

This approach is straightforward but can be optimized. For a more efficient solution, you might consider a different method that combines the sum calculation and validation in a single traversal.


## Example
Let's break down the dry run of the Sum Tree algorithm using the provided example with an animated-like step-by-step explanation. Here’s how the process would look:

### Tree Structure

```
        10
       /  \
      20   30
     /  \
    10   10
```

### Initial Call

1. **Start:**
   - The function `isSumTree(root)` is called with the root node (`10`).

### Step 1: Calculate Left and Right Subtree Sums

2. **Calculate Sum of Left Subtree:**
   - We call `sum(root->left)`, where `root->left` is `20`.

#### Calculate Sum of Subtree Rooted at `20`

3. **Initialization:**
   - Start with an empty queue and enqueue node `20`.

4. **Traverse the Subtree:**
   - **Dequeue Node `20`:**
     - Add `20` to the sum.
     - Enqueue its children: `10` and `10`.
   - **Queue Now:** `[10, 10]`
   - **Sum So Far:** `20`

   - **Dequeue Node `10` (Left Child of `20`):**
     - Add `10` to the sum.
     - No children to enqueue.
   - **Queue Now:** `[10]`
   - **Sum So Far:** `20 + 10 = 30`

   - **Dequeue Node `10` (Right Child of `20`):**
     - Add `10` to the sum.
     - No children to enqueue.
   - **Queue Now:** `[]`
   - **Sum So Far:** `30 + 10 = 40`

   - **Sum of the Subtree Rooted at `20` is `40`.**

5. **Calculate Sum of Right Subtree:**
   - We call `sum(root->right)`, where `root->right` is `30`.

#### Calculate Sum of Subtree Rooted at `30`

6. **Initialization:**
   - Start with an empty queue and enqueue node `30`.

7. **Traverse the Subtree:**
   - **Dequeue Node `30`:**
     - Add `30` to the sum.
     - No children to enqueue.
   - **Queue Now:** `[]`
   - **Sum So Far:** `30`

   - **Sum of the Subtree Rooted at `30` is `30`.**

### Step 2: Check if Node Value Equals Sum of Subtrees

8. **Comparison for Root Node (`10`):**
   - Calculate the sum of the left and right subtree sums: `40 (left) + 30 (right) = 70`.
   - Compare `10` with `70`.

   - **Check:** `10` (node value) != `70` (sum of subtrees).

### Final Decision

9. **Result:**
   - Since `10` is not equal to `70`, the tree does not satisfy the sum tree property.

10. **Return Value:**
    - The function `isSumTree(root)` returns `false`, indicating that the tree is not a sum tree.

### Summary

- **Node Value:** `10`
- **Left Subtree Sum:** `40`
- **Right Subtree Sum:** `30`
- **Total Sum of Subtrees:** `70`
- **Comparison:** `10 != 70`
- **Result:** `false`

This step-by-step animation illustrates how each node is processed to determine whether the binary tree adheres to the sum tree property.

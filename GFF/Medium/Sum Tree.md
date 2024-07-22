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

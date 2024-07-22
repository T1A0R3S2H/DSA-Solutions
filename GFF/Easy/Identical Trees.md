# Method 1 (TC: O(n+m), SC: O(1))
## Explanation

### Problem Statement
The problem is to check if two binary trees are identical or not. Two binary trees are considered identical if they have the same structure and their corresponding nodes have the same values.

### Approach
The given method `isIdentical` uses a recursive approach to compare the two binary trees.

1. **Base Case:** 
   - If both nodes (`r1` and `r2`) are `NULL`, then they are identical (since both are empty).
   - If one of the nodes is `NULL` and the other is not, then the trees are not identical.

2. **Recursive Case:**
   - If both nodes are not `NULL`, then:
     - Compare the data of the current nodes (`r1->data` and `r2->data`).
     - Recursively check the left children of both nodes (`isIdentical(r1->left, r2->left)`).
     - Recursively check the right children of both nodes (`isIdentical(r1->right, r2->right)`).
   - If the data of the current nodes match and both the left and right subtrees are identical, return `true`. Otherwise, return `false`.

Here's the code with detailed comments:

```cpp
class Solution
{
public:
    // Function to check if two trees are identical.
    bool isIdentical(Node *r1, Node *r2)
    {
        // Base case: both nodes are NULL, the trees are identical
        if (r1 == NULL && r2 == NULL)
            return true;
        
        // If one node is NULL and the other is not, the trees are not identical
        if (r1 == NULL || r2 == NULL)
            return false;
        
        // If both nodes are not NULL, check their data and recursively check their subtrees
        if (r1 != NULL && r2 != NULL)
        {
            return (r1->data == r2->data) && // Check current node's data
                   isIdentical(r1->left, r2->left) && // Recursively check left subtrees
                   isIdentical(r1->right, r2->right); // Recursively check right subtrees
        }
        
        // This point is never reached because all cases are handled above
        return false;
    }
};
```

### Time Complexity (TC)
- Each node in both trees is visited once.
- Let `n` be the number of nodes in the first tree and `m` be the number of nodes in the second tree.
- In the worst case, we visit all `n + m` nodes.
- Hence, the time complexity is \( O(n + m) \).

### Space Complexity (SC)
- The space complexity is determined by the maximum depth of the recursion stack.
- In the worst case, the depth of the recursion stack is equal to the height of the tree.
- For a balanced binary tree, the height is \( O(\log n) \), but for a skewed tree, the height can be \( O(n) \).
- Since we are not using any additional data structures apart from the recursion stack, the space complexity is \( O(h) \) where `h` is the height of the tree.
- Therefore, in the worst case, the space complexity is \( O(n) \).

### Summary
- **Time Complexity:** \( O(n + m) \)
- **Space Complexity:** \( O(h) \) where `h` is the height of the tree (worst case \( O(n) \))

# SKEWED TREE (EXTRA)
A skewed tree is a type of binary tree where all the nodes are either to the left or to the right. This means that the tree essentially forms a linear structure, resembling a linked list. There are two types of skewed trees:

1. **Left-Skewed Tree:** Every node has only a left child (or no child). This means that all the nodes are connected through the left child pointers.
2. **Right-Skewed Tree:** Every node has only a right child (or no child). This means that all the nodes are connected through the right child pointers.

### Examples

#### Left-Skewed Tree
```
    10
   /
  9
 /
8
/
...
```

#### Right-Skewed Tree
```
10
  \
   9
    \
     8
      \
      ...
```

### Properties
- **Height:** The height of a skewed tree is equal to the number of nodes, \( n \), because every node only contributes to one side of the tree.
- **Time Complexity:** Operations like insertion, deletion, and search can take \( O(n) \) time in a skewed tree, as you might need to traverse all the nodes.
- **Space Complexity:** The space complexity for recursive operations (like traversal) on a skewed tree can also be \( O(n) \) due to the depth of the recursion stack.

### Comparison with Balanced Tree
In contrast, a balanced binary tree (like a perfect binary tree or a complete binary tree) has a height of \( O(\log n) \), which leads to much more efficient operations (insertion, deletion, search) with time complexity \( O(\log n) \).

### Code
```cpp
class Solution
{
public:
    void findLongestPath(Node* root, int currentLength, int currentSum, int& maxLength, int& maxSum) {
    if (root == NULL) {
        return;
    }

    currentLength++;
    currentSum += root->data;

    if (root->left == NULL && root->right == NULL) {
        if (currentLength > maxLength) {
            maxLength = currentLength;
            maxSum = currentSum;
        } else if (currentLength == maxLength && currentSum > maxSum) {
            maxSum = currentSum;
        }
    }

    findLongestPath(root->left, currentLength, currentSum, maxLength, maxSum);
    findLongestPath(root->right, currentLength, currentSum, maxLength, maxSum);
}

int sumOfLongRootToLeafPath(Node* root) {
    int maxLength = 0;
    int maxSum = 0;
    findLongestPath(root, 0, 0, maxLength, maxSum);
    return maxSum;
}
};
```
### Explanation

1. **Function `findLongestPath`**:
   - **Base Case**: If the current node (`root`) is `NULL`, return. This means we've reached the end of a path in the tree.
   - **Increment Path Length and Sum**: Increase the `currentLength` by 1 and add the current node's value (`root->data`) to `currentSum`.
   - **Check if Leaf Node**: If the current node is a leaf (both left and right children are `NULL`):
     - Compare the `currentLength` with `maxLength`. If `currentLength` is greater, update `maxLength` and `maxSum`.
     - If `currentLength` equals `maxLength`, compare `currentSum` with `maxSum`. Update `maxSum` if `currentSum` is greater.
   - **Recursive Calls**: Call `findLongestPath` recursively for the left and right children of the current node, passing the updated `currentLength` and `currentSum`.

2. **Function `sumOfLongRootToLeafPath`**:
   - Initialize `maxLength` and `maxSum` to 0.
   - Call `findLongestPath` with the root node and initial values of `currentLength` and `currentSum` as 0.
   - Return `maxSum`, which will be the sum of the nodes on the longest path from root to a leaf.

### Time Complexity

- The time complexity of this solution is **O(n)**, where `n` is the number of nodes in the tree. This is because we visit each node exactly once during the traversal.

### Space Complexity

- The auxiliary space complexity is **O(h)**, where `h` is the height of the tree. This is due to the recursive call stack. In the worst case (for a skewed tree), this could be **O(n)**, but for a balanced tree, it would be **O(log n)**.

### Dry Run

Let's perform a dry run of the code with the following example tree:

```
         4        
       /   \       
      2     5      
     / \   / \     
    7   1 2   3    
       /
      6
```

1. **Initial Call**:
   - `sumOfLongRootToLeafPath(root)` is called.
   - `maxLength` = 0, `maxSum` = 0.
   - `findLongestPath(root, 0, 0, maxLength, maxSum)` is called with the root node (4).

2. **Traversal**:
   - Visit node 4: `currentLength` = 1, `currentSum` = 4.
   - Recur on left child (node 2):
     - Visit node 2: `currentLength` = 2, `currentSum` = 6.
     - Recur on left child (node 7):
       - Visit node 7: `currentLength` = 3, `currentSum` = 13.
       - Node 7 is a leaf. Update `maxLength` = 3, `maxSum` = 13.
     - Recur on right child (node 1):
       - Visit node 1: `currentLength` = 3, `currentSum` = 7.
       - Recur on left child (node 6):
         - Visit node 6: `currentLength` = 4, `currentSum` = 13.
         - Node 6 is a leaf. Update `maxLength` = 4, `maxSum` = 13.
   - Recur on right child (node 5):
     - Visit node 5: `currentLength` = 2, `currentSum` = 9.
     - Recur on left child (node 2):
       - Visit node 2: `currentLength` = 3, `currentSum` = 11.
       - Node 2 is a leaf. No update since `currentLength` < `maxLength`.
     - Recur on right child (node 3):
       - Visit node 3: `currentLength` = 3, `currentSum` = 12.
       - Node 3 is a leaf. No update since `currentLength` < `maxLength`.

3. **Return Result**:
   - `sumOfLongRootToLeafPath` returns `maxSum` = 13.

Thus, the sum of nodes on the longest path from root to leaf in the given tree is 13.

### Code
```cpp
class BSTIterator {
    stack<TreeNode *> st;
public:
    BSTIterator(TreeNode *root) {
        pushAll(root);
    }

    /** @return whether we have a next smallest number */
    bool hasNext() {
        return !st.empty();
    }

    /** @return the next smallest number */
    int next() {
        TreeNode *tmpNode=st.top();
        st.pop();
        pushAll(tmpNode->right);
        return tmpNode->val;
    }

private:
    void pushAll(TreeNode *node) {
        for (; node != NULL; st.push(node), node=node->left);
    }
};

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator* obj = new BSTIterator(root);
 * int param_1 = obj->next();
 * bool param_2 = obj->hasNext();
 */
```

### Explanation

The `BSTIterator` class is designed to provide an iterator for a Binary Search Tree (BST), allowing in-order traversal. The in-order traversal visits nodes in ascending order (left-root-right). The class utilizes a stack to simulate the recursive in-order traversal process iteratively.

- **Stack `st`**: The stack stores pointers to `TreeNode` objects. It helps keep track of nodes that need to be visited.
  
- **Constructor**: The constructor initializes the stack by calling the `pushAll(root)` function. This function pushes the root and all its left descendants onto the stack, ensuring that the smallest element (the leftmost node) is ready to be accessed first.

- **`hasNext()`**: This method checks if there are any nodes left to visit. It returns `true` if the stack is not empty, indicating more elements to traverse.

- **`next()`**: This method returns the next smallest element in the BST:
  1. It pops the top node from the stack, which is the current smallest node.
  2. It then calls `pushAll(tmpNode->right)`, pushing all the left children of the right subtree onto the stack, ensuring that the next smallest node is on top of the stack.
  3. Finally, it returns the value of the node.

- **`pushAll(TreeNode *node)`**: This private helper method pushes all the left descendants of a given node onto the stack. It ensures that the smallest node in the current subtree is always on top of the stack.

### Time Complexity

- **`hasNext()`**: The time complexity of `hasNext()` is \(O(1)\) since it only checks if the stack is empty.

- **`next()`**: The time complexity of `next()` is \(O(1)\) on average for each call. While it might seem that pushing nodes onto the stack in the `pushAll()` method takes \(O(h)\) time (where \(h\) is the height of the tree), over the entire traversal, each node is pushed and popped from the stack once. Thus, across all operations, the average time per operation is \(O(1)\).

### Space Complexity

- The space complexity of the `BSTIterator` is \(O(h)\), where \(h\) is the height of the tree. This is due to the space required by the stack to store nodes. In the worst case (a skewed tree), \(h\) can be equal to the number of nodes \(n\), leading to \(O(n)\) space. In a balanced tree, \(h = O(\log n)\).

### Dry Run

Let's go through a dry run with a sample BST:

```
      7
     / \
    3   15
       /  \
      9    20
```

#### Initialization

- **Constructor**: `BSTIterator(root)` is called with `root` being the node with value `7`.
- **Stack after `pushAll(root)`**: The stack contains `[7, 3]` (7 pushed first, then 3).

#### Operations

1. **`hasNext()`**: Returns `true` since the stack is `[7, 3]`.
   
2. **`next()`**: 
   - Pops `3` from the stack (`tmpNode = 3`).
   - Stack becomes `[7]`.
   - No right child for node `3`, so no call to `pushAll()`.
   - Returns `3`.

3. **`hasNext()`**: Returns `true` since the stack is `[7]`.

4. **`next()`**:
   - Pops `7` from the stack (`tmpNode = 7`).
   - Stack becomes `[]`.
   - Calls `pushAll(tmpNode->right)`, which is `pushAll(15)`.
     - Stack becomes `[15, 9]` (15 pushed, then 9).
   - Returns `7`.

5. **`hasNext()`**: Returns `true` since the stack is `[15, 9]`.

6. **`next()`**:
   - Pops `9` from the stack (`tmpNode = 9`).
   - Stack becomes `[15]`.
   - No right child for node `9`, so no call to `pushAll()`.
   - Returns `9`.

7. **`hasNext()`**: Returns `true` since the stack is `[15]`.

8. **`next()`**:
   - Pops `15` from the stack (`tmpNode = 15`).
   - Stack becomes `[]`.
   - Calls `pushAll(tmpNode->right)`, which is `pushAll(20)`.
     - Stack becomes `[20]`.
   - Returns `15`.

9. **`hasNext()`**: Returns `true` since the stack is `[20]`.

10. **`next()`**:
    - Pops `20` from the stack (`tmpNode = 20`).
    - Stack becomes `[]`.
    - No right child for node `20`, so no call to `pushAll()`.
    - Returns `20`.

11. **`hasNext()`**: Returns `false` since the stack is empty.

### Summary

The `BSTIterator` class efficiently supports in-order traversal of a BST using an iterative approach with a stack. It ensures that the next smallest element is always ready to be accessed in constant time. The space complexity is proportional to the height of the tree, making it scalable for balanced trees.

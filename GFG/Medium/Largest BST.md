# Method 1 (not the most optimal , O(n2))
### Code
```cpp
class Solution{
    public:
    // Function to perform inorder traversal and store values in a vector
    void inorder(Node* root, vector<int>& ans) {
        if (root == nullptr) return;
        inorder(root->left, ans);
        ans.push_back(root->data);
        inorder(root->right, ans);
    }
    
    // Function to check if a tree is a BST
    bool isBST(Node* root) {
        vector<int> ans;
        inorder(root, ans);
        int n = ans.size();
        for (int i = 1; i < n; i++) {
            if (ans[i-1] >= ans[i]) {
                return false;
            }
        }
        return true;
    }
    
    // Function to calculate the size of the subtree rooted at the given node
    int countNodes(Node* root) {
        if (root == nullptr) return 0;
        return 1 + countNodes(root->left) + countNodes(root->right);
    }
    
    // Function to find the size of the largest BST in the binary tree
    int largestBst(Node* root) {
        if (root == nullptr) return 0;
        
        // Check if the current subtree is a BST
        if (isBST(root)) {
            return countNodes(root);
        }
        
        // Otherwise, find the largest BST in the left and right subtrees
        int leftLargestBST = largestBst(root->left);
        int rightLargestBST = largestBst(root->right);
    
        return max(leftLargestBST, rightLargestBST);
    }
};
```

### **Explanation**

The provided code defines a class `Solution` with the following functions:

1. **`inorder(Node* root, vector<int>& ans)`**: This function performs an inorder traversal of the tree rooted at `root` and stores the result in the vector `ans`. Inorder traversal of a BST should give a sorted sequence.

2. **`isBST(Node* root)`**: This function uses the `inorder` function to check if the tree rooted at `root` is a BST. It does this by checking if the inorder traversal results in a sorted sequence. If the sequence is sorted, the tree is a BST, and the function returns `true`; otherwise, it returns `false`.

3. **`countNodes(Node* root)`**: This function calculates the number of nodes in the subtree rooted at `root` using a simple recursive count.

4. **`largestBst(Node* root)`**: This is the main function that finds the size of the largest BST in the binary tree. It follows these steps:
   - If the current subtree is a BST (checked using `isBST`), it returns the size of the subtree using `countNodes`.
   - If it's not a BST, it recursively checks the left and right subtrees to find the largest BST and returns the maximum of these sizes.

### **Time Complexity Analysis**

- **`inorder` Function**: The time complexity of an inorder traversal is O(n), where n is the number of nodes in the subtree. This is because each node is visited exactly once.

- **`isBST` Function**: This function calls `inorder` (O(n)) and then checks if the vector is sorted (O(n)), resulting in a time complexity of O(n).

- **`countNodes` Function**: This function also visits each node exactly once, resulting in a time complexity of O(n).

- **`largestBst` Function**: 
  - For each node, it calls `isBST` (O(n)) and possibly `countNodes` (O(n)).
  - If the tree has n nodes, in the worst case, `largestBst` will be called on each node. Thus, the time complexity is O(n^2) in the worst case because each call to `isBST` takes O(n), and there are O(n) nodes.

### **Space Complexity Analysis**

- The space complexity is determined mainly by the depth of the recursive calls and the space used for the `inorder` vector.
- **Space for `inorder` vector**: O(n), as it stores the inorder traversal.
- **Recursive stack space**: O(h), where h is the height of the tree. In the worst case, the tree is skewed, making h = O(n). Thus, the space complexity can be O(n) in the worst case.

### **Dry Run**

Let's perform a dry run using the following example tree:

```
         6
       /   \
      6     2              
       \   / \
        2 1   3
```

1. **Initial Call**: `largestBst(root)` where `root` is the node with value `6`.

2. **First Call to `isBST(6)`**:
   - Inorder traversal: [6, 2, 1, 3] (Not sorted, so it is not a BST).
   - Result: `false`.

3. **Recursive Calls**:
   - Check the left subtree of the root: `largestBst(6)` (left child).
   
4. **Call to `isBST(6)` (left child)**:
   - Inorder traversal: [6, 2] (Not sorted, so it is not a BST).
   - Result: `false`.
   - Further recursive calls:
     - `largestBst(nullptr)` (left child) returns 0.
     - `largestBst(2)` (right child).

5. **Call to `isBST(2)` (right child of left 6)**:
   - Inorder traversal: [2] (Single node, so it is a BST).
   - Result: `true`.
   - `countNodes(2)` returns 1.

6. **Back to First Left Child Call**:
   - `leftLargestBST = 0`, `rightLargestBST = 1`.
   - Result: `max(0, 1) = 1`.

7. **Check the right subtree of the root: `largestBst(2)`** (right child of root):
   - Call to `isBST(2)`:
     - Inorder traversal: [1, 2, 3] (Sorted, so it is a BST).
     - Result: `true`.
     - `countNodes(2)` returns 3.

8. **Final Calculation**:
   - `leftLargestBST = 1`, `rightLargestBST = 3`.
   - Result: `max(1, 3) = 3`.

The largest BST in the given tree has a size of 3, which matches the output of the dry run.

### **Conclusion**

- The provided approach checks each subtree for the BST property and counts its size if it's a BST. While this approach works correctly, it has a higher time complexity (O(n^2)) due to the repeated traversal of nodes.
- This is why it is not optimal for larger trees (up to 105 nodes) as given in the constraints. An optimized approach using post-order traversal would be more suitable for large input sizes.

# Method 1 (op1, op2, op3) BUT TLE
```cpp
int diameter(Node* root) {
        if(root==0) return 0;
        int opt1=diameter(root->left);
        int opt2=diameter(root->right);
        int opt3=height(root->left)+1+height(root->right);
        return max(opt1, max(opt2, opt3));
    }
    
    int height(Node* root){
        if(root==0) return 0;
        return max(height(root->left), height(root->right))+1;
    }
```

# Method 2 (single traversal for diameter and height)
```cpp
int heightAndDiameter(Node* root, int& diameter) {
        if (root == nullptr) return 0;
        int leftHeight = heightAndDiameter(root->left, diameter);
        int rightHeight = heightAndDiameter(root->right, diameter);
        diameter = max(diameter, leftHeight + rightHeight + 1);
        return max(leftHeight, rightHeight) + 1;
    }

    // Function to calculate the diameter of the tree
    int diameter(Node* root) {
        int diameter = 0;
        heightAndDiameter(root, diameter);
        return diameter;
    }
```
### Explanation

The code calculates the diameter of a binary tree using a single traversal. The diameter of a tree is defined as the length of the longest path between any two nodes. To achieve this, the `heightAndDiameter` function recursively computes the height of each subtree while updating the diameter. It returns the height of the current subtree and updates the diameter based on the sum of the heights of the left and right subtrees plus one (for the current node).

The `diameter` function initializes the diameter variable to zero and invokes the `heightAndDiameter` function, which updates the diameter as it traverses the tree. By the end of the traversal, the diameter variable contains the length of the longest path in the tree. This approach ensures that each node is processed only once, making the computation efficient.

### Dry Run Example

Consider a tree with the following structure:

```
      1
     / \
    2   3
   / \
  4   5
```

1. **Start at Node 1**: Call `heightAndDiameter(1, diameter)`. Initialize diameter to 0.

2. **Traverse Left Subtree of Node 1**:
   - **At Node 2**: Call `heightAndDiameter(2, diameter)`.
     - **At Node 4**: Call `heightAndDiameter(4, diameter)`. Node 4 is a leaf, so both left and right subtree heights are 0. Diameter is updated to 1 (0 + 0 + 1). Return height 1.
     - **At Node 5**: Call `heightAndDiameter(5, diameter)`. Node 5 is a leaf, so both left and right subtree heights are 0. Diameter is updated to 1. Return height 1.
     - **Back at Node 2**: Height of left subtree (Node 4) is 1, height of right subtree (Node 5) is 1. Diameter is updated to 3 (1 + 1 + 1). Return height 2 (max(1, 1) + 1).

3. **Traverse Right Subtree of Node 1**:
   - **At Node 3**: Call `heightAndDiameter(3, diameter)`. Node 3 is a leaf, so both left and right subtree heights are 0. Diameter remains 3. Return height 1.

4. **Back at Node 1**: Height of left subtree (Node 2) is 2, height of right subtree (Node 3) is 1. Diameter is updated to 4 (2 + 1 + 1). Return height 3 (max(2, 1) + 1).

The final diameter is 4, which represents the longest path between any two nodes (4 → 2 → 1 → 3).

### Time Complexity

The time complexity is \(O(n)\), where \(n\) is the number of nodes in the tree. This is because each node is visited exactly once during the traversal.

### Space Complexity

The space complexity is \(O(h)\), where \(h\) is the height of the tree. This space is used for the recursion stack. In the worst case of a skewed tree, this space complexity can be \(O(n)\), but for a balanced tree, it is \(O(\log n)\).


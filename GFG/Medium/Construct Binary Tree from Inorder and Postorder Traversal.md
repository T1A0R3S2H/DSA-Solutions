```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int postIndex;
    
    TreeNode* buildTreeHelper(vector<int>& inorder, vector<int>& postorder, int inorderStart, int inorderEnd) {
        if (inorderStart > inorderEnd) {
            return nullptr;
        }

        TreeNode* root = new TreeNode(postorder[postIndex--]);

        int inorderIndex;
        for (int i=inorderStart;i<=inorderEnd;i++) {
            if (inorder[i]==root->val) {
                inorderIndex=i;
                break;
            }
        }

        // first right then left because in post order, R-L-N
        root->right=buildTreeHelper(inorder, postorder, inorderIndex+1, inorderEnd);
        root->left=buildTreeHelper(inorder, postorder, inorderStart, inorderIndex-1);

        return root;
    }
    
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        postIndex=postorder.size()-1;
        return buildTreeHelper(inorder, postorder, 0, inorder.size()-1);
    }
};

```

### Explanation:

#### Algorithm:

1. **Base Case:** If `inorderStart` is greater than `inorderEnd`, return `nullptr` because this means there are no elements to construct the subtree.
2. **Root Node:** Create the root node using the current value of `postorder[postIndex]`, and decrement `postIndex` to move to the next node.
3. **Find Root in Inorder Traversal:** Locate the root's index in the `inorder` array. This splits the `inorder` array into left and right subtrees.
4. **Recursive Calls:** 
   - Recursively build the right subtree first using the elements after the root index in the `inorder` array.
   - Then, recursively build the left subtree using the elements before the root index in the `inorder` array.

#### Time Complexity (TC):

- **Finding Root in Inorder Traversal:** O(n) for each node (due to the loop).
- **Building the Tree:** Each node is processed once, and the recursive calls are made for left and right subtrees.

Thus, the overall time complexity is O(n^2) in the worst case because for each of the n nodes, we might scan up to n elements to find the root index in the `inorder` array.

#### Space Complexity (SC):

- **Auxiliary Space:** O(n) for the recursion stack in the worst case when the tree is completely unbalanced.
- **Additional Space:** O(1) extra space (excluding the space required for the input and output).

Therefore, the overall space complexity is O(n).

### Dry Run:

Consider the following example:

```cpp
inorder = [9, 3, 15, 20, 7]
postorder = [9, 15, 7, 20, 3]
```

#### Step-by-Step Execution:

1. **Initial Call:**
   - `postIndex` = 4 (points to 3)
   - Create root node with value `3`.
   - Find `3` in `inorder` at index 1.

2. **Right Subtree:**
   - Call `buildTreeHelper(inorder, postorder, 2, 4)`
   - `postIndex` = 3 (points to 20)
   - Create root node with value `20`.
   - Find `20` in `inorder` at index 3.

3. **Right Subtree of 20:**
   - Call `buildTreeHelper(inorder, postorder, 4, 4)`
   - `postIndex` = 2 (points to 7)
   - Create root node with value `7`.
   - Find `7` in `inorder` at index 4.

4. **Left and Right Subtree of 7:**
   - Both calls with `inorderStart > inorderEnd` return `nullptr`.

5. **Left Subtree of 20:**
   - Call `buildTreeHelper(inorder, postorder, 2, 2)`
   - `postIndex` = 1 (points to 15)
   - Create root node with value `15`.
   - Find `15` in `inorder` at index 2.

6. **Left and Right Subtree of 15:**
   - Both calls with `inorderStart > inorderEnd` return `nullptr`.

7. **Left Subtree:**
   - Call `buildTreeHelper(inorder, postorder, 0, 0)`
   - `postIndex` = 0 (points to 9)
   - Create root node with value `9`.
   - Find `9` in `inorder` at index 0.

8. **Left and Right Subtree of 9:**
   - Both calls with `inorderStart > inorderEnd` return `nullptr`.

### Final Tree Structure:

```
       3
      / \
     9   20
        /  \
       15   7
```

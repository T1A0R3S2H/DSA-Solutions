```cpp
class Solution {
public:
    int preindex = 0;  // To keep track of the current root in preorder array

    Node* solve(int pre[], int in[], int inorderStart, int inorderEnd, int n) {
        // Base case: if the starting index exceeds the ending index
        if (inorderStart > inorderEnd) {
            return nullptr;
        }

        // Create the root node with the current value from preorder array
        Node* root = new Node(pre[preindex++]);

        // Find the index of the root in the inorder array
        int inorderIndex;
        for (int i = inorderStart; i <= inorderEnd; i++) {
            if (in[i] == root->data) {
                inorderIndex = i;
                break;
            }
        }

        // Recursively build the left and right subtrees
        root->left = solve(pre, in, inorderStart, inorderIndex - 1, n);
        root->right = solve(pre, in, inorderIndex + 1, inorderEnd, n);

        return root;  // Return the constructed subtree root
    }

    Node* buildTree(int in[], int pre[], int n) {
        // Initialize starting indices for inorder traversal
        return solve(pre, in, 0, n - 1, n);  // Call the helper function
    }
};
```

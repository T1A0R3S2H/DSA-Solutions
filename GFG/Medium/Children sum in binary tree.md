### Code
```cpp
class Solution {
public:
    // Function to check whether all nodes of a tree have the value 
    // equal to the sum of their child nodes.
    int isSumProperty(Node *root) {
        // Base case: if the node is null or it is a leaf node, return true
        if (root==nullptr || (root->left==nullptr && root->right==nullptr)) {
            return 1; // Returning 1 (true) since there's nothing to check
        }

        int left_data=0, right_data=0;

        // If left child exists, assign its data
        if (root->left) {
            left_data=root->left->data;
        }

        // If right child exists, assign its data
        if (root->right) {
            right_data=root->right->data;
        }

        // Check the sum property at the current node and recurse for children
        if (root->data==left_data+right_data &&
            isSumProperty(root->left) && 
            isSumProperty(root->right)) {
            return 1;
        } 
        else {
            return 0;
        }
    }
};
```

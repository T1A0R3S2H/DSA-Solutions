Your approach to finding the kth ancestor in a binary tree is correct. You can traverse the tree from the root to the given node and store the path in a vector. Once the path is found, you can easily determine the kth ancestor by accessing the appropriate element in the vector.

Here's how you can implement this approach in C++:

1. Perform a depth-first search (DFS) to find the path from the root to the given node and store the nodes in a vector.
2. Once you have the path, if the kth ancestor exists, return the corresponding element from the vector; otherwise, return -1.

Here is the complete implementation:

```cpp
#include <iostream>
#include <vector>
using namespace std;

struct Node {
    int data;
    Node *left, *right;
    Node(int val) {
        data = val;
        left = right = NULL;
    }
};

// Helper function to find the path from the root to the given node
bool findPath(Node* root, vector<int> &path, int node) {
    if (root == NULL) {
        return false;
    }
    
    // Add current node to the path
    path.push_back(root->data);
    
    // Check if the current node is the target node
    if (root->data == node) {
        return true;
    }
    
    // Recursively check the left and right subtrees
    if ((root->left && findPath(root->left, path, node)) || 
        (root->right && findPath(root->right, path, node))) {
        return true;
    }
    
    // If the target node is not found, remove the current node from the path
    path.pop_back();
    return false;
}

int kthAncestor(Node *root, int k, int node) {
    vector<int> path;
    
    // Find the path from the root to the given node
    if (!findPath(root, path, node)) {
        return -1;
    }
    
    // The kth ancestor is the (size-k-1)th element in the path
    int index = path.size() - k - 1;
    if (index >= 0) {
        return path[index];
    }
    
    return -1;
}

// Helper function to create a binary tree for testing
Node* createTree() {
    Node* root = new Node(1);
    root->left = new Node(2);
    root->right = new Node(3);
    root->left->left = new Node(4);
    root->left->right = new Node(5);
    root->right->left = new Node(6);
    root->right->right = new Node(7);
    return root;
}

int main() {
    Node* root = createTree();
    int k = 2, node = 4;
    cout << "Kth ancestor of node " << node << " is: " << kthAncestor(root, k, node) << endl;

    k = 1;
    node = 3;
    cout << "Kth ancestor of node " << node << " is: " << kthAncestor(root, k, node) << endl;

    return 0;
}
```

This implementation includes:
- A helper function `findPath` to find and store the path from the root to the given node.
- The `kthAncestor` function to calculate and return the kth ancestor.
- A `createTree` function to set up a sample binary tree for testing purposes.

You can modify the `createTree` function to create different binary trees for further testing.

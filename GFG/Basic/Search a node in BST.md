```cpp
bool search(Node* root, int x) {
    if (root==NULL) return NULL;
        if (root->data==x) return root;
        if (root->data>x) {
            return search(root->left, x);
        } 
        else {
            return search(root->right, x);
        }
}
```

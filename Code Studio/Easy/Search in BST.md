```cpp
bool searchInBST(BinaryTreeNode<int> *root, int x) {
    if(root==nullptr) return false;
    if(root->data==x) return true;
    if (root->data>x){
        return searchInBST(root->left, x);
    }
    if (root->data<x){
        return searchInBST(root->right, x);
    }
}
```

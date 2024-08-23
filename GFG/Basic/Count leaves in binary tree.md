### Code:
```cpp
void inorder(Node* root, vector<int>& result) {
    if (root!=nullptr) {
        inorder(root->left, result);
        result.push_back(root->data);
        inorder(root->right, result);
    }
}

int countLeaves(Node* root) {
    if (root==nullptr) return 0;
    if (root->left==nullptr && root->right==nullptr) return 1;
    return countLeaves(root->left)+countLeaves(root->right);
}

int noofleaves(Node* root) {
    int count=0;
    if (root==nullptr) return 0;
    if (root->left==nullptr && root->right==nullptr) return 1;
    count+=noofleaves(root->left);
    count+=noofleaves(root->right);

```

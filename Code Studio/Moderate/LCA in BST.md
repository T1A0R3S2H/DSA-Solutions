```cpp
TreeNode *LCAinaBST(TreeNode *root, TreeNode *P, TreeNode *Q)
{
    if(root==nullptr) {
        return root;
    }
    
    if(root->data==P->data || root->data==Q->data) {
        return root;
    }
    
    TreeNode* LEFTSIDE=LCAinaBST(root->left, P, Q);
    TreeNode* RIGHTSIDE=LCAinaBST(root->right, P, Q);
    
    if(LEFTSIDE!=nullptr && RIGHTSIDE!=nullptr) {
        return root;
    } 
    
    else if(LEFTSIDE!=nullptr) {
        return LEFTSIDE;
    }
    
    else {
        return RIGHTSIDE;
    }
}

```

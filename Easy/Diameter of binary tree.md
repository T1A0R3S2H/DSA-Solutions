# Method 1 (Op1, Op2, Op3)
```cpp
class Solution {
public:
    int diameterOfBinaryTree(TreeNode* root) {
        if(root==0) return 0;
        int opt1=diameterOfBinaryTree(root->left);
        int opt2=diameterOfBinaryTree(root->right);
        int opt3=height(root->left)+height(root->right);
        return max(opt1, max(opt2, opt3));
    }
    
    int height(TreeNode* root){
        if(root==0) return 0;
        return max(height(root->left), height(root->right))+1;
    }
};
```

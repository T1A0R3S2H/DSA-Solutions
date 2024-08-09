```cpp
#include<bits/stdc++.h>
/*************************************************************

    Following is the Binary Tree node structure
    class TreeNode
    {
    public:
        int data;
        TreeNode *left, *right;
        TreeNode() : data(0), left(NULL), right(NULL) {}
        TreeNode(int x) : data(x), left(NULL), right(NULL) {}
        TreeNode(int x, TreeNode *left, TreeNode *right) : data(x), left(left), right(right) {}
    };

*************************************************************/
void inorder(TreeNode* root, vector<int> &vec) {
    if(root==NULL) return;
    inorder(root->left, vec);
    vec.push_back(root->data);
    inorder(root->right, vec);
}

pair<int, int> predecessorSuccessor(TreeNode *root, int key)
{
    vector<int> vec;
    inorder(root, vec);
    int pre=-1;
    int suc=-1;
    
    for(int i=0;i<vec.size();i++){
        if (vec[i]<key) {
            pre=vec[i];
        }
        if (vec[i]>key) {
            suc=vec[i];
            break;
        }
    }
    
    return {pre, suc};
}
```

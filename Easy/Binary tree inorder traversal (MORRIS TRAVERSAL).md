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
    vector<int> inorderTraversal(TreeNode *root)
{

    vector<int> v;
    TreeNode *cur = root;
    TreeNode *prev = NULL;
    TreeNode *temp = NULL;

  // run algorithm until root exists
    while (cur)
    {
        // if left child exists
        if (cur->left)
        {
            prev=cur->left; // catch hold of left child
           
            while(prev->right) // go to right most node of left child
                prev=prev->right;
        
            prev->right=cur; // make it point to the root

            temp=cur;
            cur=cur->left;

            temp->left=NULL; // make left of root null
        }
        else
        {
            v.push_back(cur->val); // print the node value
            cur=cur->right; // move to right child (because left child doesn't exist)
        }
    }

    return v;
}
};
```

```

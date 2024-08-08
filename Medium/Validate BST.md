# Method 1 (finding range of each node)  `WHY WRONG?? ASK`
```cpp
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        return isBST(root, INT_MIN, INT_MAX);
    }

    bool isBST(TreeNode*root, int min, int max){
        if (root==nullptr) return true;
        if (root->val>min && root->val<max){
            return isBST(root->left, min, root->val) && isBST(root->right, root->val, max);
        }
        else{
            return false;
        }
    }
};
```
![image](https://github.com/user-attachments/assets/3d1c51dc-7db6-4a72-a009-ea484742b9fa)



# Method 2 (using inorder traversal)
```cpp
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        vector<int> ans;
        inorder(root, ans);
        int n=ans.size();
        for (int i=1;i<n;i++) {
            if (ans[i-1]>=ans[i]) {
                return false;
            }
        }
        return true;
    }

    void inorder(TreeNode* root, vector<int>& ans) {
        if (root==nullptr) return;
        inorder(root->left, ans);
        ans.push_back(root->val);
        inorder(root->right, ans);
    }
};

```

```cpp
class Solution {
public:
    bool findTarget(TreeNode* root, int k) {
        vector<int> nums;
        inorder(root, nums); // to get sorted array
    
        // two pointer approach
        int left=0, right=nums.size()-1;
        while (left<right) {
            int sum=nums[left]+nums[right];
            if (sum==k) return true;
            if (sum<k) left++;
            else right--;
        }
        return false;
    }

    void inorder(TreeNode* root, vector<int>& nums) {
        if (!root) return;
        inorder(root->left, nums);
        nums.push_back(root->val);
        inorder(root->right, nums);
    }
};
```

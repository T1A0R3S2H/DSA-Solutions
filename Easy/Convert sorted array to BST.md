```cpp
class Solution {
public:
    // Logic -> divide array in 2 parts --> create new node with arr[mid] then root -> start to mid - 1 and root -> right = mid + 1 to end.

    // TC = O(N), SC = O(Height) approx O(log n)

    TreeNode *solve(vector<int>& nums, int start , int end){
        if(start>end){
            return NULL;
        }
        int mid=start+(end-start)/2;

        // crete node with value nums[mid]
        TreeNode *root=new TreeNode(nums[mid]);
        root->left=solve(nums, start, mid-1);
        root->right=solve(nums, mid+1, end);
        return root ;
    }

    TreeNode* sortedArrayToBST(vector<int>& nums) {
        TreeNode *root=solve(nums,0,nums.size()-1);
        return root ;
    }
};
```

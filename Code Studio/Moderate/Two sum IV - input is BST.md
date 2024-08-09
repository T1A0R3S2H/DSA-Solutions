```cpp
void inorder(BinaryTreeNode<int>* root, vector<int>& nums) {
        if (!root) return;
        inorder(root->left, nums);
        nums.push_back(root->data);
        inorder(root->right, nums);
}

bool twoSumInBST(BinaryTreeNode<int>* root, int target) {
	    vector<int> nums;
        inorder(root, nums); // to get sorted array
    
        // two pointer approach
        int left=0, right=nums.size()-1;
        while (left<right) {
            int sum=nums[left]+nums[right];
            if (sum==target) return true;
            if (sum<target) left++;
            else right--;
        }
        return false;
}

```

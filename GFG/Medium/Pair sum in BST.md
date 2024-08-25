```cpp
class Solution{
  public:
    // root : the root Node of the given BST
    // target : the target sum
    int isPairPresent(struct Node *root, int target){
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
    
    void inorder(Node* root, vector<int>& nums) {
        if (!root) return;
        inorder(root->left, nums);
        nums.push_back(root->data);
        inorder(root->right, nums);
    }
};
```

```cpp
class Solution {
  public:
  
    Node *solve(vector<int>& nums, int start , int end){
        if(start>end){
            return NULL;
        }
        int mid=start+(end-start)/2;

        // crete node with value nums[mid]
        Node *root=new Node(nums[mid]);
        root->left=solve(nums, start, mid-1);
        root->right=solve(nums, mid+1, end);
        return root ;
    }
    
    Node* sortedArrayToBST(vector<int>& nums) {
        Node *root=solve(nums,0,nums.size()-1);
        return root;
    }
    
    
};
```

```bas
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int n=nums.size();
        int max1=INT_MIN;
        int max2=INT_MIN;
        
        // Find the two maximum values in the array
        for (int num:nums) {
            if (num>max1) {
                max2=max1;
                max1=num;
            } 
            else if (num>max2) {
                max2=num;
            }
        }
        
        // Calculate the maximum product
        return (max1-1)*(max2-1);
    }
};
```

```cpp
class Solution {
  public:
    // Function to find the sum of contiguous subarray with maximum sum.
    int maxSubarraySum(vector<int> &arr) {
        int cursum=arr[0], maxsum=arr[0];
        int n= arr.size();
        for(int i=1; i<n; i++){
            cursum=max(arr[i], cursum+arr[i]);
            maxsum=max(maxsum, cursum);
        }
        return maxsum;
    }
};
```

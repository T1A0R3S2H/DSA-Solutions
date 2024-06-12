## Method 1
The code aims to find the length of the longest subarray with a sum of 0 in the given array A. It initializes an unordered map `sumi` to store the cumulative sum as the key and the index where that sum was first encountered as the value. The code iterates through the array A, updating the cumulative sum `sum` at each index `i`. If the `sum` becomes 0, it means the subarray from the start to the current index has a sum of 0, so the maximum length `maxi` is updated to the current index plus one. If the `sum` already exists in the `sumi` map, it indicates that a subarray with sum 0 has been found, and the length of that subarray is calculated as `i - sumi[sum]`, where `i` is the current index, and `sumi[sum]` is the index where the sum was first encountered; this correctly gives the length of the subarray with sum 0 ending at index `i`. The `maxi` variable is then updated to the maximum of the current `maxi` and this length. If the `sum` does not exist in the map, it is added to the map with the current index `i` as its value, to store the index where this sum was first encountered for future reference. Finally, the maximum length `maxi` is returned, which represents the length of the longest subarray with sum 0 in the given array `A`.
```bash
class Solution{
    public:
    int maxLen(vector<int>&A, int n)
    {   
        int maxi=0;
        unordered_map<int, int> sumi;
        int sum=0;
        for (int i=0;i<A.size();i++){
            sum+=A[i];
            if (sum==0){
                maxi=i+1;
            }
            else if (sumi.find(sum)!=sumi.end()){
                maxi=max(maxi, i-sumi[sum]);
            }
            else {
                sumi[sum]=i;
            }
        }
        return maxi;
    }
};
```

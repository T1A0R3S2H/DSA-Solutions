```cpp
class Solution{
public:
    //Function to find the days of buying and selling stock for max profit.
    vector<vector<int>> stockBuySell(vector<int> A, int n) {
        vector<vector<int>> ans;
        if(n <= 1) return ans; 
        int minPrice = A[0];
        int minIndex = 0;
        for(int i = 1; i < n; i++) {
            if (A[i] > minPrice) {
                ans.push_back({minIndex, i});
            }
            minPrice = A[i];
            minIndex = i;
    }
    return ans;
    }
};
```

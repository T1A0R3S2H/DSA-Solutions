## Method 1
- We initialize two variables: `minPrice` to keep track of the minimum price encountered so far, and `maxProfit` to keep track of the maximum profit that can be obtained.
- We iterate through the prices array.
- For each price, we update the `minPrice` by taking the minimum of the current `minPrice` and the current price.
- After updating the `minPrice`, we check if we can get a better profit by selling the stock at the current price. We do this by calculating the difference between the current price and the `minPrice`. If this difference is greater than the current `maxProfit`, we update `maxProfit`.
- After iterating through the entire array, maxProfit will contain the maximum profit that can be obtained by buying and selling the stock once.
##
```bash
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int minPrice=INT_MAX;
        int maxProfit=0;
        
        for (int i=0;i<prices.size();i++) {
            minPrice=min(minPrice, prices[i]);
            maxProfit = max(maxProfit, prices[i] - minPrice);
        }
        
        return maxProfit;
    }
};
```
##
- Time Complexity: O(n), where n is the length of the prices array, as we iterate through the array once.
- Space Complexity: O(1), as we use a constant amount of extra space to store the minPrice and maxProfit variables.

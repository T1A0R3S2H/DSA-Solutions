![Screenshot 2024-06-27 130132.png](https://assets.leetcode.com/users/images/c72b9d67-be8c-4926-9ce5-b00a5a76eb35_1719473634.192599.png)


# Code
```cpp
class Solution {
public:
    int candy(vector<int>& ratings) {
        int n = ratings.size();
        if (n == 0) return 0; // Handle edge case of empty ratings
        
        vector<int> allocate(n, 1); // Start with 1 candy for each child
        
        // First pass: from left to right
        for (int i = 1; i < n; ++i) {
            if (ratings[i] > ratings[i - 1]) {
                allocate[i] = allocate[i - 1] + 1;
            }
        }
        
        // Second pass: from right to left
        for (int i = n - 2; i >= 0; --i) {
            if (ratings[i] > ratings[i + 1]) {
                allocate[i] = max(allocate[i], allocate[i + 1] + 1);
            }
        }
        
        // Calculate total candies
        int totalCandies = 0;
        for (int candy : allocate) {
            totalCandies += candy;
        }
        
        return totalCandies;
    }
};

```

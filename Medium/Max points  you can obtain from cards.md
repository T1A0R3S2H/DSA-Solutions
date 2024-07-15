```cpp
class Solution {
public:
    int maxScore(vector<int>& cardPoints, int k) {
        int lSum = 0; // Sum of the first k elements from the left
        int rSum = 0; // Sum of the elements from the right that will be added
        long long maxSum = 0; // Maximum score
        // Calculate initial sum of the first k elements from the left
        for (int i = 0; i < k; ++i) {
            lSum += cardPoints[i];
        }
        maxSum = lSum; // Initialize maxSum with the sum of the first k elements
        int rIndex = cardPoints.size() - 1; // Start index for summing elements from the right
        // Iterate from the last element of the left sum to the first
        for (int i = k - 1; i >= 0; --i) {
            lSum -= cardPoints[i]; // Remove the last element from the left sum
            rSum += cardPoints[rIndex]; // Add the current element from the right
            rIndex -= 1; // Move to the next element from the right
            maxSum = max(maxSum, (long long)(lSum + rSum)); // Update maxSum
        }
        
        return maxSum;
    }
};
```

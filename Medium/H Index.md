# H-Index Calculation Explanation

## Understanding the H-Index
The H-index is a metric used to measure the productivity and citation impact of a researcher. It is defined as the maximum value of \( h \) such that the given author has published \( h \) papers that have each been cited at least \( h \) times.

## Approach Explanation

### Step 1: Count the Frequency of Citations
- We start by counting the frequency of each citation number using an unordered map (hash map). This helps us quickly look up how many papers have a specific number of citations.
- Initialize an unordered map `freq` to store the frequency of each citation count.

### Step 2: Iterate Through the Citations
- Iterate through each citation in the input list and update the frequency map.

### Step 3: Check for H-Index
- We need to check for the H-index from the maximum possible value (which is the total number of papers) down to 0.
- For each possible H-index value (from `n` to 0), count the number of papers that have citations greater than or equal to the current H-index.
- Initialize a counter `count` to keep track of the number of papers that meet the criteria for the current H-index.

### Step 4: Return the H-Index
- If the count of papers with citations greater than or equal to the current H-index is at least as large as the H-index itself, then we have found our H-index and return it.

### Example
Consider the citations list: `[3, 0, 6, 1, 5]`

1. Count the frequency of each citation:
   - `freq = {3: 1, 0: 1, 6: 1, 1: 1, 5: 1}`

2. Check possible H-index values from 5 to 0:
   - For `h = 5`, count papers with citations >= 5: There are 2 papers (6 and 5). Since 2 < 5, continue.
   - For `h = 4`, count papers with citations >= 4: There are 2 papers (6 and 5). Since 2 < 4, continue.
   - For `h = 3`, count papers with citations >= 3: There are 3 papers (3, 6, 5). Since 3 >= 3, we have found our H-index which is 3.

Thus, the H-index for the given citations list is 3.

### Code
```cpp
class Solution {
public:
    int hIndex(vector<int>& citations) {
        int n=citations.size();
        int count=0;
        unordered_map<int, int> freq;
        for (int num:citations) {
            freq[num]++;
        }

        // Check for h-index from n to 0
        for (int h=n;h>=0;h--) {
            count=0;
            for (const auto& pair:freq) {
                if (pair.first>=h) {
                    count+=pair.second;
                }
            }
            if (count>=h) {
                return h;
            }
        }
        
        return 0;
    }
};
```

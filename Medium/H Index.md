# Method 1 (unordered map)

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

### Eg.
Consider the citations list: `[3, 0, 6, 1, 5]`

1. Count the frequency of each citation:
   - `freq = {3: 1, 0: 1, 6: 1, 1: 1, 5: 1}`

2. Check possible H-index values from 5 to 0:
   - For `h = 5`, count papers with citations >= 5: There are 2 papers (6 and 5). Since 2 < 5, continue.
   - For `h = 4`, count papers with citations >= 4: There are 2 papers (6 and 5). Since 2 < 4, continue.
   - For `h = 3`, count papers with citations >= 3: There are 3 papers (3, 6, 5). Since 3 >= 3, we have found our H-index which is 3.

Thus, the H-index for the given citations list is 3.

### C++ Code
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

# Method 2 (brute force)
The H-index is a metric that measures the impact and productivity of a researcher based on their number of publications and the citations each publication receives. The goal is to find the maximum value \( h \) such that the researcher has at least \( h \) papers that have been cited at least \( h \) times.

## Problem Statement
## Steps to Calculate H-Index

1. **Sort the Citations Array:**
   - Sort the array of citations in ascending order.

2. **Iterate Through the Sorted Array:**
   - Initialize `n` as the size of the citations array.
   - Iterate through the sorted array and check the condition \( n - i \leq \text{citations}[i] \):
     - If the condition is true, return \( n - i \).
     - If the loop completes without finding such a value, return 0.

## Example

Consider the following example to illustrate the H-index calculation:

**Input:** `citations = [3, 0, 6, 1, 5]`

**Steps:**
1. **Sort the citations array:** `citations = [0, 1, 3, 5, 6]`
2. **Initialize `n`** as the size of the citations array (`n = 5`).
3. **Iterate through the sorted array:**
   - For \( i = 0 \): \( 5 - 0 = 5 \leq 0 \) (False)
   - For \( i = 1 \): \( 5 - 1 = 4 \leq 1 \) (False)
   - For \( i = 2 \): \( 5 - 2 = 3 \leq 3 \) (True)

   When the condition \( n - i \leq \text{citations}[i] \) holds true, return \( n - i \). Here, \( n - i = 3 \), so the H-index is 3.

## Key Points

- **Sorting the Array:** Sorting ensures that we can compare the number of citations each paper has with its position in the sorted list.
- **Condition Check:** The loop checks the condition \( n - i \leq \text{citations}[i] \) to find the maximum \( h \) such that there are \( h \) papers with at least \( h \) citations.
- **Efficiency:** This approach ensures that the algorithm runs efficiently and correctly identifies the H-index.

### Code
```cpp
#include <vector>
#include <algorithm>

using namespace std;

class Solution {
public:
    int hIndex(vector<int>& citations) {
        int n = citations.size();
        sort(citations.begin(), citations.end());

        for (int i = 0; i < n; ++i) {
            if (n - i <= citations[i]) {
                return n - i;
            }
        }
        return 0;
    }
};

```

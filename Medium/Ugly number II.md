# Method 1 (brute force)
```cpp
class Solution {
public:
    int nthUglyNumber(int n) {
        vector<int> t(n + 1); //To handle edge cases
        t[1] = 1; // First ugly number
        // Initialize t2, t3 & t5 with 1 to pointing the 1st ugly number
        int t2 = 1;
        int t3 = 1;
        int t5 = 1;
        for (int i = 2; i <= n; i++) {
            int second = t[t2] * 2;
            int third = t[t3] * 3;
            int fifth = t[t5] * 5;
            t[i] = min({second, third, fifth});
            if (t[i] == second) {
                t2++;
            }
            if (t[i] == third) {
                t3++;
            }
            if (t[i] == fifth) {
                t5++;
            }
        }
        return t[n];
    }
};
```
This code is solving the problem of finding the n-th "ugly number." An ugly number is a number that only has prime factors 2, 3, and 5. The sequence starts with 1, and the next few numbers are formed by multiplying the previous ugly numbers by 2, 3, or 5.

### Explanation with an Example

Let's say `n = 10`. We want to find the 10th ugly number.

### Initial Setup

- `t` is a vector of size `n+1` to store ugly numbers.
- `t[1] = 1` initializes the first ugly number as 1.
- `t2`, `t3`, and `t5` are indices initialized to 1, pointing to the first ugly number in the sequence.

### Iteration Logic

The code iterates from 2 to `n` and calculates the next ugly number in each iteration.

For each iteration:
1. **Calculate Candidates:**  
   `second = t[t2] * 2`  
   `third = t[t3] * 3`  
   `fifth = t[t5] * 5`  
   These represent the next possible ugly numbers by multiplying the smallest available ugly numbers by 2, 3, or 5.

2. **Select the Minimum:**  
   `t[i] = min({second, third, fifth})`  
   The minimum of these three candidates is selected as the next ugly number and stored in `t[i]`.

3. **Update Indices:**  
   Depending on which candidate was chosen, the corresponding index (`t2`, `t3`, or `t5`) is incremented to point to the next ugly number.

### Example Walkthrough

Let's go through the first few iterations:

- **Iteration 1:**  
  `t[1] = 1` (already set)

- **Iteration 2:**  
  `second = 1 * 2 = 2`  
  `third = 1 * 3 = 3`  
  `fifth = 1 * 5 = 5`  
  `t[2] = min(2, 3, 5) = 2`  
  Increment `t2` to 2.

- **Iteration 3:**  
  `second = 2 * 2 = 4`  
  `third = 1 * 3 = 3`  
  `fifth = 1 * 5 = 5`  
  `t[3] = min(4, 3, 5) = 3`  
  Increment `t3` to 2.

- **Iteration 4:**  
  `second = 2 * 2 = 4`  
  `third = 2 * 3 = 6`  
  `fifth = 1 * 5 = 5`  
  `t[4] = min(4, 6, 5) = 4`  
  Increment `t2` to 3.

- **Iteration 5:**  
  `second = 3 * 2 = 6`  
  `third = 2 * 3 = 6`  
  `fifth = 1 * 5 = 5`  
  `t[5] = min(6, 6, 5) = 5`  
  Increment `t5` to 2.

This process continues until the 10th ugly number is calculated.

### Result
For `n = 10`, the sequence of ugly numbers would be [1, 2, 3, 4, 5, 6, 8, 9, 10, 12], so the 10th ugly number is 12. The function would return `t[10] = 12`.


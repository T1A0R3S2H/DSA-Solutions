## Method 1 (Optimal Approach)
```
        // we travel through the array and when we encounter a larger element (break point)
        // than the first element, then we swap the larger element and the one
        // previous to it and if the array is arranged in decreasing order, then (no break point, edge case)
        // reverse the array if largest element is at the beginning, then except
        // it continue the above process
```
```bash
class Solution {
public:
    void nextPermutation(vector<int>& A) {
    int n = A.size(); // size of the array.

    // Step 1: Find the break point, 1s digit to 10s, 100s, and so on
    int ind = -1; // break point
    for (int i = n - 2; i >= 0; i--) {
        if (A[i] < A[i + 1]) {
            // index i is the break point
            ind = i;
            break;
        }
    }

    // EDGE CASE: If break point does not exist
    // It means the array was already sorted in desc order
    if (ind == -1) {
        // reverse the whole array:
        reverse(A.begin(), A.end());
        return ;
    }

    // Step 2: Find the next greater element in rightPart
    for (int i = n - 1; i > ind; i--) {
        if (A[i] > A[ind]) {
            swap(A[i], A[ind]);
            break;
        }
    }

    // Step 3: reverse the rightPart
    reverse(A.begin() + ind + 1, A.end());
    }
};
```


## Method 2 (Using in-built function)
![image](https://github.com/T1A0R3S2H/Leetcode-Progess/assets/123285559/0b0f0df3-1bfa-4628-855e-98c67361bdf2)


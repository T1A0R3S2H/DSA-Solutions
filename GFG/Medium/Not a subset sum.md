### Code:
```cpp
class Solution {
public:
    long long findSmallest(vector<int> &arr) {
        long long res = 1;
        for (int i = 0; i < arr.size(); i++) {
            if (arr[i] > res) {
                break;
            }
            res += arr[i];
        }
        return res;
    }
};
```

1. Initialize `res = 1` (the smallest possible sum we're looking for).
2. Iterate through the sorted array.
3. For each element:
   - If it's greater than `res`, we've found our answer.
   - Otherwise, add it to `res`. This means we can now form all sums up to and including the new `res`.
4. After the loop, `res` is our answer.

Time Complexity: O(n)
- We iterate through the array once, performing constant time operations for each element.

Space Complexity: O(1)
- We only use a single variable `res` regardless of the input size.

Detailed Dry Run:
Let's use the example: arr = [1, 2, 3]

1. Initialize res = 1

2. First iteration (i = 0):
   - arr[0] = 1
   - Is 1 > res (1)? No.
   - res = res + arr[0] = 1 + 1 = 2
   - Meaning: We can now form sums 1 and 2 (1, 1+1)

3. Second iteration (i = 1):
   - arr[1] = 2
   - Is 2 > res (2)? No.
   - res = res + arr[1] = 2 + 2 = 4
   - Meaning: We can now form sums 1, 2, 3, and 4 (1, 2, 1+2, 1+2+1)

4. Third iteration (i = 2):
   - arr[2] = 3
   - Is 3 > res (4)? No.
   - res = res + arr[2] = 4 + 3 = 7
   - Meaning: We can now form sums 1, 2, 3, 4, 5, 6 (1, 2, 3, 1+2, 1+3, 2+3, 1+2+3)

5. Loop ends, return res = 7

Why 7 is the correct answer:
- Using [1, 2, 3], we can form all sums from 1 to 6.
- 7 is the smallest number that cannot be formed by any combination of these elements.

The algorithm works because at each step, `res` represents the smallest number we can't form yet. When we add the next element, if it's not greater than `res`, we can now form all numbers up to `res + that element`.

This approach is efficient because it leverages the sorted nature of the input array and the fact that if we can form all sums up to x, adding the next element y allows us to form all sums up to x + y.

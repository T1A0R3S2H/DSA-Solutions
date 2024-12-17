# Method 1 (using set, w/o DP):
---

### Code:
```cpp
class Solution {
public:
    int lenLongestFibSubseq(vector<int>& A) {
        int N = A.size();
        unordered_set<int> S(A.begin(), A.end());

        int ans = 0;
        for (int i = 0; i < N; ++i)
            for (int j = i+1; j < N; ++j) {
                /* With the starting pair (A[i], A[j]),
                 * y represents the future expected value in
                 * the fibonacci subsequence, and x represents
                 * the most current value found. */
                int x = A[j], y = A[i] + A[j];
                int length = 2;
                while (S.find(y) != S.end()) {
                    int z = x + y;
                    x = y;
                    y = z;
                    ans = max(ans, ++length);
                }
            }

        return ans;
    }
};
```

---

### Explanation:

This method finds the **longest Fibonacci-like subsequence** from the given strictly increasing array `A`. A Fibonacci-like subsequence satisfies the property that for any three consecutive elements `x1, x2, x3`, the condition `x1 + x2 == x3` must hold.

1. **Input Array Conversion to Set**:
   - We first convert the array `A` into an unordered set `S`. This is done for fast lookups, allowing us to check in constant time whether a number exists in the array.

2. **Outer Loops for Pairs**:
   - We iterate over each pair `(A[i], A[j])` with `i < j` using two nested loops. These two elements are treated as the first two numbers of the Fibonacci-like subsequence.

3. **Fibonacci Sequence Generation**:
   - For each pair `(A[i], A[j])`, we calculate the next expected Fibonacci-like number `y = A[i] + A[j]`.
   - We continue searching for this number `y` in the set `S` to extend the subsequence. If `y` is found, we update `x` and `y` (to the next Fibonacci-like numbers) and increment the length of the subsequence.
   - This process continues until no further Fibonacci-like number can be found in the set.

4. **Track Maximum Length**:
   - We keep track of the longest Fibonacci-like subsequence found during the iterations using the variable `ans`.

5. **Return the Result**:
   - Finally, the maximum length of the Fibonacci-like subsequence is returned.

---

### Time Complexity:

The time complexity of this approach is determined by the two main parts: the nested loops and the while loop.

1. **Outer Loops (O(N^2))**:
   - The nested loops iterate over all pairs `(i, j)` such that `i < j`. For an array of size `N`, this results in `O(N^2)` iterations.

2. **While Loop (O(log M))**:
   - For each pair `(A[i], A[j])`, the while loop tries to extend the Fibonacci-like subsequence by continuously checking the next expected number `y = x + y` in the set `S`. Since the Fibonacci sequence grows exponentially, the number of iterations in the while loop is bounded by `O(log M)`, where `M` is the maximum value in the array. This is because after a few iterations, the Fibonacci numbers will exceed `M`, and the sequence cannot continue.

3. **Set Operations (O(1))**:
   - Each set lookup `S.find(y)` takes constant time \(O(1)\) on average, because we are using an unordered set for fast lookups.

Thus, the overall time complexity is:

\[
O(N^2 \times \log M)
\]

Where:
- \(N^2\) comes from the two nested loops iterating over all pairs in `A`.
- \(\log M\) comes from the maximum number of Fibonacci-like numbers that can be generated, where the Fibonacci numbers grow exponentially.

---

### Space Complexity:

1. **Set S (O(N))**:
   - We store the elements of the array in an unordered set `S`, which requires \(O(N)\) space, where \(N\) is the number of elements in `A`.

2. **Other Variables (O(1))**:
   - We use only a constant amount of extra space for the variables `x`, `y`, `length`, and `ans`.

Thus, the overall space complexity is:

\[
O(N)
\]

---

### Dry Run:

Let's consider the array: `A = [1, 2, 3, 4, 5, 6, 7, 8]`.

#### Step-by-Step Dry Run:

- **Outer loop** starts with `i = 0`, `j = 1` (pair `(1, 2)`):
  - Next expected Fibonacci number: `y = 1 + 2 = 3`.
  - `3` is found in the set `S`. So, the subsequence becomes `[1, 2, 3]`.
  - Next expected number: `5` (`y = 2 + 3`), found in `S`. So, the subsequence becomes `[1, 2, 3, 5]`.
  - Next expected number: `8` (`y = 3 + 5`), found in `S`. So, the subsequence becomes `[1, 2, 3, 5, 8]`.
  - The length of this subsequence is 5, so `ans` is updated to 5.

- For other pairs `(1, 3)`, `(1, 4)`, and so on, no Fibonacci-like subsequence longer than 5 is found.

#### Final Output:

The longest Fibonacci-like subsequence found is `[1, 2, 3, 5, 8]` with length `5`. Thus, the output is:

```
5
```

---

### Summary:

- **Time Complexity**: \(O(N^2 \times \log M)\), where \(N\) is the length of the array, and \(M\) is the maximum value in the array.
- **Space Complexity**: \(O(N)\) for the unordered set storing the elements of the array.
- **Dry Run**: Step-by-step, we iterate over all pairs of elements and find the longest Fibonacci-like subsequence by checking the next expected numbers in the set.

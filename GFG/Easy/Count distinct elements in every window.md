### Code
```cpp
class Solution {
  public:
    vector<int> countDistinct(int A[], int n, int k) {
        vector<int> result;
        unordered_map<int, int> mp;
        
        // make frequency map for the first window
        for (int i=0; i<k; i++) {
            mp[A[i]]++;
        }
        
        // Add the count of distinct elements in the first window to the result
        result.push_back(mp.size());
        
        // for the rest of the elements
        for (int i=k; i<n; i++) {
            // Remove the element that is sliding out of the window
            if (mp[A[i-k]]==1) {
                mp.erase(A[i-k]);
            } 
            else {
                mp[A[i-k]]--;
            }
            
            // Add the new element that is sliding into the window
            mp[A[i]]++;
            
            // Add the count of distinct elements in the current window to the result
            result.push_back(mp.size());
        }
        
        return result;
    }
};
```
### Explanation

The problem requires counting the distinct elements in every window of size `K` for a given array. The solution uses a sliding window technique along with an `unordered_map` to keep track of the frequency of elements in the current window.

1. **Initialization**:
   - A vector `result` is initialized to store the count of distinct elements for each window.
   - An `unordered_map<int, int> mp` is used to maintain the frequency of elements in the current window.

2. **First Window**:
   - The first loop initializes the frequency map for the first window of size `K` (i.e., `A[0]` to `A[K-1]`).
   - The size of the map, which represents the count of distinct elements, is added to the `result`.

3. **Sliding the Window**:
   - For each new element entering the window (from `A[K]` to `A[n-1]`):
     - The element that is sliding out of the window (`A[i-K]`) is removed or its count is decremented in the map.
     - The new element (`A[i]`) entering the window is added or its count is incremented in the map.
     - The count of distinct elements (the size of the map) is recorded in the `result`.

4. **Return Result**:
   - The vector `result`, containing the count of distinct elements for each window, is returned.

### Time Complexity Analysis

- **Initialization of the first window**: This takes O(K) time to populate the frequency map.
- **Sliding the window**: Each insertion and deletion operation in the `unordered_map` takes O(1) on average. The sliding operation happens N-K times, making it O(N).
- **Total Time Complexity**: O(K) + O(N-K) = O(N).

### Space Complexity Analysis

- **Space for the result vector**: O(N-K+1), which stores the count of distinct elements for each window.
- **Space for the frequency map**: In the worst case, O(K) (if all elements in a window are distinct).
- **Total Space Complexity**: O(N) for both the result vector and the frequency map.

### Dry Run

Let's do a dry run using the first example provided:

- **Input**: `N = 7, K = 4`, `A[] = {1, 2, 1, 3, 4, 2, 3}`
- **Expected Output**: `3 4 4 3`

**Step-by-step Execution:**

1. **Initialization**:
   - `result = []`
   - `mp = {}`

2. **First Window (`i = 0` to `i = 3`)**:
   - `mp = {1: 2, 2: 1, 3: 1}` (Counts: 1 appears twice, 2 and 3 appear once)
   - `result = [3]` (3 distinct elements)

3. **Sliding Window**:

   - **`i = 4`**:
     - Element going out: `A[0] = 1`
     - Element coming in: `A[4] = 4`
     - Update `mp`: `mp = {1: 1, 2: 1, 3: 1, 4: 1}` (4 distinct elements)
     - `result = [3, 4]`

   - **`i = 5`**:
     - Element going out: `A[1] = 2`
     - Element coming in: `A[5] = 2`
     - Update `mp`: `mp = {1: 1, 2: 1, 3: 1, 4: 1}` (Still 4 distinct elements, as `2` replaced `2`)
     - `result = [3, 4, 4]`

   - **`i = 6`**:
     - Element going out: `A[2] = 1`
     - Element coming in: `A[6] = 3`
     - Update `mp`: `mp = {1: 0, 2: 1, 3: 2, 4: 1}` (3 distinct elements, `1` removed)
     - `result = [3, 4, 4, 3]`

4. **Final Output**: `[3, 4, 4, 3]`

This matches the expected output, verifying that the algorithm is working correctly.

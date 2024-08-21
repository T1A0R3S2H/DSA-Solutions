# Method 1 (brute force)
```cpp
class Solution {
  public:
    int kthLargest(vector<int> &arr, int k) {
        int n = arr.size();
        vector<int> subarray_sums;
        for (int i = 0; i < n; ++i) {
            int curr_sum = 0;
            for (int j = i; j < n; ++j) {
                curr_sum += arr[j];
                subarray_sums.push_back(curr_sum);
            }
        }
    
        // Sort the sums in descending order
        sort(subarray_sums.begin(), subarray_sums.end(), greater<int>());
    
        // Return the k-th largest sum
        return subarray_sums[k - 1];
        }
};
```


# Method 2 (min heap)
```cpp
class Solution {
  public:
    int kthLargest(vector<int> &arr, int k) {
        priority_queue<int, vector<int>, greater<int>>mini;
        int n=arr.size();
        for(int i=0;i<n;i++){
            int sum=0;
            for(int j=i;j<n;j++){
                sum+=arr[j];
                if(mini.size()<k){
                    mini.push(sum);
                }
                else{
                    if(sum>mini.top()){
                        mini.pop();
                        mini.push(sum);
                    }
                }
            }
        }
        return mini.top();
    }
};
```
### Explanation
1. **Objective:**
   - Find the K-th largest sum of contiguous subarrays within a given array.

2. **Approach:**
   - Use two nested loops to iterate over all possible subarrays.
   - For each subarray, calculate its sum and manage these sums using a min-heap (priority queue) to keep track of the top K largest sums.
   - The heap is maintained to have at most K elements. If the heap size exceeds K, the smallest element is removed.
   - After processing all subarrays, the K-th largest sum will be at the top of the min-heap.

### Time Complexity
- **Outer Loop:** Runs `n` times (where `n` is the length of the array).
- **Inner Loop:** Runs up to `n` times for each iteration of the outer loop, leading to `O(n^2)` subarray sums.
- **Heap Operations:** Each insertion and removal operation in the heap takes `O(log k)` time. Since we handle `n^2` sums and the heap operations are logarithmic in nature, the overall time complexity is `O(n^2 * log k)`.

### Space Complexity
- **Heap:** Stores up to `k` elements, thus requiring `O(k)` additional space.
- **Auxiliary Space:** Apart from the heap, no significant additional space is used, so the space complexity is `O(k)`.

### Dry Run
Let's use the example `arr = [3, 2, 1]` and `k = 2`.

1. **Initialization:**
   - `mini` is an empty min-heap.
   - `n = 3` (length of the array).

2. **Processing:**
   - **Subarrays starting at index 0:**
     - Subarray `[3]`: Sum = 3. `mini` becomes `[3]`.
     - Subarray `[3, 2]`: Sum = 5. `mini` becomes `[3, 5]` (heap size is less than k).
     - Subarray `[3, 2, 1]`: Sum = 6. `mini` is updated to `[5, 6]` (replaces 3 with 6 as 6 > 3).

   - **Subarrays starting at index 1:**
     - Subarray `[2]`: Sum = 2. Heap remains `[5, 6]` (2 is smaller than the smallest element in the heap).
     - Subarray `[2, 1]`: Sum = 3. Heap remains `[5, 6]`.

   - **Subarrays starting at index 2:**
     - Subarray `[1]`: Sum = 1. Heap remains `[5, 6]`.

3. **Final Heap State:**
   - `mini` contains `[5, 6]`.

4. **Result:**
   - The K-th largest sum (2nd largest) is at the top of the heap: `5`.

In the final output, the function correctly returns `5`, which is the 2nd largest sum of contiguous subarrays for the given example.

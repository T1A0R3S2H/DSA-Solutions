# Method 1 (sort)
```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        return nums[nums.size()-k];
    }
};


//sort and return the kth element from the end
```


# Method 2 (priority queue | heap)
```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int> pq(nums.begin(), nums.end());
        for (int i=0; i<k-1; i++) {
            pq.pop();
        }
        return pq.top();
    }
};
```
### Explanation

1. **Initialization**: The function takes in a vector `nums`, which contains the elements of the array, and an integer `k`, which represents the k-th largest element we want to find.
  
2. **Max-Heap Construction**: A max-heap (priority queue) `pq` is created and initialized with all the elements of the `nums` vector. In C++, the `priority_queue` by default is a max-heap, meaning it will always have the largest element at the top.

3. **Extracting Elements**: To find the k-th largest element, we need to remove the largest element from the heap `k-1` times. This is done using a loop that calls `pq.pop()` `k-1` times.

4. **Returning the Result**: After `k-1` elements are removed, the top element of the heap is the k-th largest element. The function returns this value using `pq.top()`.

### Time Complexity Analysis

- **Heap Construction**: Constructing the heap from `n` elements takes \( O(n) \) time.
- **Removing k-1 Elements**: Each `pop()` operation takes \( O(\log n) \) time due to heap reordering. Since we perform this operation `k-1` times, this part of the code takes \( O((k-1) \log n) \) time.

Therefore, the overall time complexity is \( O(n + k \log n) \).

### Space Complexity Analysis

- The space complexity is \( O(n) \) because we are storing all elements of the array in the max-heap.

### Dry Run

Let's dry run the code with a small example:

- **Input**: `nums = [3, 2, 1, 5, 6, 4]`, `k = 2`

1. **Heap Initialization**:
   - The max-heap is constructed with all elements: `pq = [6, 5, 4, 3, 2, 1]`
   
2. **Pop Elements**:
   - First iteration (`i = 0`): `pq.pop()` removes `6`, resulting in `pq = [5, 3, 4, 1, 2]`
   
3. **Get the k-th Largest Element**:
   - Now, `pq.top()` returns `5`, which is the 2nd largest element in the array.

- **Output**: `5`

The code correctly identifies `5` as the 2nd largest element in the given example.

### Summary

- The solution uses a max-heap to efficiently find the k-th largest element by removing the largest element `k-1` times and then checking the top of the heap.
- The time complexity is \( O(n + k \log n) \), which is efficient for large arrays when k is small relative to n.
- The space complexity is \( O(n) \), which is required to store the elements in the heap.

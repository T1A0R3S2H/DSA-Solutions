# Method 1: Using a Min-Heap (Priority Queue)
```cpp
class Solution {
  public:
    int kthSmallest(vector<int> &arr, int k) {
        // Step 1: Create a min-heap (priority queue)
        priority_queue<int, vector<int>, greater<int>> pq(arr.begin(), arr.end());

        // Step 2: Pop k-1 elements from the min-heap
        for(int i=0; i<k-1; i++) {
            pq.pop();
        }

        // Step 3: The top element is the kth smallest element
        return pq.top();
    }
};
```

#### **Explanation:**
- **Step 1:** A min-heap is created from the elements of the array `arr`. In a min-heap, the smallest element is always at the top.
- **Step 2:** The smallest element is removed from the heap `k-1` times. After these removals, the smallest element remaining in the heap is the `k`th smallest element in the original array.
- **Step 3:** The top element of the heap (after `k-1` removals) is returned, which is the `k`th smallest element.

#### **Time Complexity:**
- **Building the heap:** `O(n)` where `n` is the number of elements in the array.
- **Extracting the min `k-1` times:** `O(k log n)` because each `pop` operation takes `O(log n)` time.
- **Total:** `O(n + k log n)`.

#### **Space Complexity:**
- **Heap space:** `O(n)` because the heap stores all elements of the array.

#### **Dry Run:**
- **Input:** `arr = [7, 10, 4, 3, 20, 15]`, `k = 3`
- **Heap after Step 1:** `[3, 7, 4, 10, 20, 15]` (Heap order may vary internally, but `3` is at the top)
- **After 1st pop:** `[4, 7, 15, 10, 20]`
- **After 2nd pop:** `[7, 10, 15, 20]`
- **Top element:** `7`, which is the 3rd smallest element.

---

# Method 2: Using a Frequency Array
```cpp
int kthSmallest(std::vector<int> &arr, int k) {
    int max_element = *std::max_element(arr.begin(), arr.end());

    // Step 1: Create a frequency array
    std::vector<int> count(max_element + 1, 0);

    // Step 2: Populate the frequency array
    for (int i = 0; i < arr.size(); i++) {
        count[arr[i]]++;
    }

    // Step 3: Traverse the frequency array to find the k-th smallest element
    int cumulative_count = 0;
    for (int i = 1; i <= max_element; i++) {
        cumulative_count += count[i];
        if (cumulative_count >= k) {
            return i;
        }
    }

    return -1; // This line should never be reached if k is valid
}
```

#### **Explanation:**
- **Step 1:** Find the maximum element in the array, then create a frequency array `count` of size `max_element + 1`. Initialize all values to 0.
- **Step 2:** Traverse the input array and increment the corresponding index in the frequency array for each element. This records the frequency of each element.
- **Step 3:** Traverse the frequency array. Keep a cumulative count of elements. When this cumulative count equals or exceeds `k`, the current index is the `k`th smallest element in the array.

#### **Time Complexity:**
- **Finding the maximum element:** `O(n)`
- **Populating the frequency array:** `O(n)`
- **Traversing the frequency array:** `O(max_element)`
- **Total:** `O(n + max_element)`.

#### **Space Complexity:**
- **Frequency array:** `O(max_element)`.

#### **Dry Run:**
- **Input:** `arr = [7, 10, 4, 3, 20, 15]`, `k = 3`
- **Step 1:** `max_element = 20`
- **Step 2:** `count = [0, 0, 0, 1, 1, ..., 0, 1, 0, 1, 1, 0, 1]` (Indices 3, 4, 7, 10, 15, 20 have 1s)
- **Step 3:** Cumulative count during traversal:
  - At `i = 3`, `cumulative_count = 1`
  - At `i = 4`, `cumulative_count = 2`
  - At `i = 7`, `cumulative_count = 3`
  - **Return `7` as the 3rd smallest element**.

---

Both methods effectively solve the problem but are suited to different scenarios. The min-heap approach is more general, while the frequency array approach leverages constraints on the input to achieve a potentially faster solution with the given constraints.

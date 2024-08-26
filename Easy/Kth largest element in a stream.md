### Code
```cpp
class KthLargest {
    int k;
    priority_queue<int, vector<int>, greater<int>> minHeap;

public:
    KthLargest(int k, vector<int>& nums) {
        this->k = k;
        for (int num : nums) {
            add(num);
        }
    }
    
    int add(int val) {
        if (minHeap.size() < k) {
            minHeap.push(val);
        }
        else if (val > minHeap.top()) { // new value is larger than the smallest in the heap
            minHeap.pop(); // Remove the smallest value
            minHeap.push(val); // Insert the new value
        }
        return minHeap.top(); // The kth largest element is at the top of the min-heap
    }
};
```

### Explanation

1. **Class Members**:
   - `k`: This integer stores the required k-th largest element.
   - `minHeap`: A min-heap (using `priority_queue` with `greater<int>` comparator) to store the `k` largest elements from the stream of numbers.

2. **Constructor (`KthLargest(int k, vector<int>& nums)`)**:
   - Initializes `k` to the given value.
   - Initializes the min-heap with the k largest elements from the initial stream `nums` by calling the `add` method for each element. This ensures the heap only contains the `k` largest elements after processing.

3. **Method (`int add(int val)`)**:
   - If the heap size is less than `k`, the new value `val` is pushed directly into the heap.
   - If the heap already contains `k` elements, it compares `val` with the smallest element in the heap (the top of the min-heap). If `val` is larger, it removes the smallest element and inserts `val` to keep only the `k` largest elements.
   - The method returns the smallest element in the heap, which represents the k-th largest element in the stream.

### Time Complexity Analysis

1. **Initialization**:
   - Building the heap from the `nums` array takes `O(n log k)` time, where `n` is the number of elements in `nums`. Each insertion into the heap takes `O(log k)` time, and this operation is performed `n` times.

2. **Add Method**:
   - Each call to `add()` involves either pushing an element into the heap or replacing the smallest element (if the new element is larger). Both these operations take `O(log k)` time due to heap operations (insert and remove).

### Space Complexity Analysis

- The space complexity is `O(k)` because the min-heap stores at most `k` elements, regardless of the size of the input stream.

### Dry Run

Let's perform a dry run using the provided example:

#### Input:
```cpp
["KthLargest", "add", "add", "add", "add", "add"]
[[3, [4, 5, 8, 2]], [3], [5], [10], [9], [4]]
```

#### Execution:

1. **Initialization**:
   - `KthLargest kthLargest = new KthLargest(3, [4, 5, 8, 2]);`
     - `k = 3`
     - Call `add(4)`: Heap becomes `[4]` (size < k)
     - Call `add(5)`: Heap becomes `[4, 5]` (size < k)
     - Call `add(8)`: Heap becomes `[4, 5, 8]` (size = k)
     - Call `add(2)`: 2 is not added since it's smaller than the min-heap top (4).
     - Final heap after initialization: `[4, 5, 8]`

2. **`kthLargest.add(3)`**:
   - Heap = `[4, 5, 8]`
   - `3 < 4` (heap top), so 3 is not added.
   - Return `4` (the k-th largest element).

3. **`kthLargest.add(5)`**:
   - Heap = `[4, 5, 8]`
   - `5 == 4` (heap top), so replace 4 with 5.
   - Heap after replacement: `[5, 5, 8]`
   - Return `5` (the k-th largest element).

4. **`kthLargest.add(10)`**:
   - Heap = `[5, 5, 8]`
   - `10 > 5` (heap top), replace 5 with 10.
   - Heap after replacement: `[5, 10, 8]`
   - Return `5` (the k-th largest element).

5. **`kthLargest.add(9)`**:
   - Heap = `[5, 10, 8]`
   - `9 > 5` (heap top), replace 5 with 9.
   - Heap after replacement: `[8, 10, 9]`
   - Return `8` (the k-th largest element).

6. **`kthLargest.add(4)`**:
   - Heap = `[8, 10, 9]`
   - `4 < 8` (heap top), so 4 is not added.
   - Return `8` (the k-th largest element).

#### Final Output: `[null, 4, 5, 5, 8, 8]`

This dry run shows how the heap maintains the k-th largest element efficiently, and the implementation meets the problem's requirements.

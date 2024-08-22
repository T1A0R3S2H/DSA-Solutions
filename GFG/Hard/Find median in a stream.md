### Code:
```cpp
class Solution {
    priority_queue<int> maxHeap; // Max-heap for the smaller half
    priority_queue<int, vector<int>, greater<int>> minHeap; // Min-heap for the larger half

public:
    // Function to insert into heaps.
    void insertHeap(int &x) {
        if (maxHeap.empty() || x <= maxHeap.top()) {
            maxHeap.push(x);
        } 
        else {
            minHeap.push(x);
        }
        
        balanceHeaps();
    }

    // Function to balance the heaps.
    void balanceHeaps() {
        if (maxHeap.size() > minHeap.size() + 1) {
            minHeap.push(maxHeap.top());
            maxHeap.pop();
        } 
        else if (minHeap.size() > maxHeap.size()) {
            maxHeap.push(minHeap.top());
            minHeap.pop();
        }
    }

    // Function to return the median.
    double getMedian() {
        if (maxHeap.size()>minHeap.size()) {
            return maxHeap.top();
        } 
        else if(maxHeap.size()==minHeap.size()) {
            return (maxHeap.top() + minHeap.top()) / 2.0;
        }
    }
};
```

### Explanation:
This solution uses two heaps to maintain the elements seen so far:
- **Max-Heap (`maxHeap`)**: This heap stores the smaller half of the elements. The top of this heap is the largest element in this half.
- **Min-Heap (`minHeap`)**: This heap stores the larger half of the elements. The top of this heap is the smallest element in this half.

#### Workflow:
1. **Insertion (`insertHeap`)**:
   - If `maxHeap` is empty or the new element is less than or equal to the top of `maxHeap`, it's added to `maxHeap`.
   - Otherwise, it's added to `minHeap`.
   - After insertion, `balanceHeaps` is called to ensure the heaps are balanced.

2. **Balancing Heaps (`balanceHeaps`)**:
   - If `maxHeap` has more than one extra element compared to `minHeap`, move the top of `maxHeap` to `minHeap`.
   - If `minHeap` has more elements than `maxHeap`, move the top of `minHeap` to `maxHeap`.
   - This ensures that the difference in the size of the heaps is at most 1.

3. **Finding the Median (`getMedian`)**:
   - If `maxHeap` has more elements, the median is the top of `maxHeap`.
   - If both heaps have the same number of elements, the median is the average of the tops of both heaps.

### Time Complexity (TC):
- **Insertion and Balancing:** Both insertion into a heap and balancing the heaps (which involves moving the top element from one heap to another) take `O(log N)` time. Since we perform these operations for each of the `N` elements, the overall time complexity is `O(N log N)`.

- **Finding the Median:** Finding the median is an `O(1)` operation since it involves checking the top elements of the heaps.

- **Overall Time Complexity:** `O(N log N)`.

### Space Complexity (SC):
- The space complexity is `O(N)` because we store all the elements in the two heaps.

### Example 1:
Let's dry run the code with an example: `X[] = {5, 15, 1, 3}`.

1. **Insert 5**:
   - `maxHeap`: `{5}`
   - `minHeap`: `{}`
   - Median: `5` (since `maxHeap` has more elements).

2. **Insert 15**:
   - `maxHeap`: `{5}`
   - `minHeap`: `{15}`
   - Median: `(5 + 15) / 2 = 10` (since both heaps have the same number of elements).

3. **Insert 1**:
   - `maxHeap`: `{5, 1}`
   - `minHeap`: `{15}`
   - After balancing:
     - `maxHeap`: `{5, 1}`
     - `minHeap`: `{15}`
   - Median: `5` (since `maxHeap` has more elements).

4. **Insert 3**:
   - `maxHeap`: `{5, 1}`
   - `minHeap`: `{15, 3}`
   - After balancing:
     - `maxHeap`: `{3, 1}`
     - `minHeap`: `{15, 5}`
   - Median: `(3 + 5) / 2 = 4` (since both heaps have the same number of elements).

Output sequence: `5, 10, 5, 4`.

This dry run shows how the heaps evolve with each insertion and how the median is determined efficiently.


### Example 2:
Let's use the array `X[] = {10, 20, 30, 40, 50, 60}`.

### Initial State:
- `maxHeap`: `{}`
- `minHeap`: `{}`

### Insert 10:
- Insert 10 into `maxHeap` because `maxHeap` is empty.
  - `maxHeap`: `{10}`
  - `minHeap`: `{}`
- **Median:** `10` (since `maxHeap` has more elements).

### Insert 20:
- Insert 20 into `minHeap` because 20 is greater than the top of `maxHeap` (which is 10).
  - `maxHeap`: `{10}`
  - `minHeap`: `{20}`
- **Median:** `(10 + 20) / 2 = 15.0` (since both heaps have the same number of elements).

### Insert 30:
- Insert 30 into `minHeap` because 30 is greater than the top of `maxHeap` (which is 10).
  - `maxHeap`: `{10}`
  - `minHeap`: `{20, 30}`
- **Balancing:** Since `minHeap` has more elements than `maxHeap`, move the top of `minHeap` (which is 20) to `maxHeap`.
  - `maxHeap`: `{20, 10}`
  - `minHeap`: `{30}`
- **Median:** `20` (since `maxHeap` has more elements).

### Insert 40:
- Insert 40 into `minHeap` because 40 is greater than the top of `maxHeap` (which is 20).
  - `maxHeap`: `{20, 10}`
  - `minHeap`: `{30, 40}`
- **Median:** `(20 + 30) / 2 = 25.0` (since both heaps have the same number of elements).

### Insert 50:
- Insert 50 into `minHeap` because 50 is greater than the top of `maxHeap` (which is 20).
  - `maxHeap`: `{20, 10}`
  - `minHeap`: `{30, 40, 50}`
- **Balancing:** Since `minHeap` has more elements than `maxHeap`, move the top of `minHeap` (which is 30) to `maxHeap`.
  - `maxHeap`: `{30, 10, 20}`
  - `minHeap`: `{40, 50}`
- **Median:** `30` (since `maxHeap` has more elements).

### Insert 60:
- Insert 60 into `minHeap` because 60 is greater than the top of `maxHeap` (which is 30).
  - `maxHeap`: `{30, 10, 20}`
  - `minHeap`: `{40, 50, 60}`
- **Median:** `(30 + 40) / 2 = 35.0` (since both heaps have the same number of elements).

### Final State of Heaps:
- **`maxHeap`:** `{30, 10, 20}`
- **`minHeap`:** `{40, 50, 60}`

### Final Median Outputs:
- For each insertion, the median would be: `10, 15.0, 20, 25.0, 30, 35.0`.

### Summary:
- **Insertion of 50 and 60** resulted in a situation where the difference between `maxHeap` and `minHeap` was temporarily more than 1, prompting balancing operations.
- The heaps remained balanced by moving elements between them as necessary, ensuring that the difference in their sizes never exceeds 1.
- The median calculation worked seamlessly by either taking the top of the larger heap or averaging the tops of both heaps when they were of equal size.

This example illustrates how the solution handles cases where the difference between the sizes of the heaps exceeds 1 and balances them correctly to maintain the efficient median calculation.

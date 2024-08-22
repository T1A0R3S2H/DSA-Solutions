```cpp
class MedianFinder {
    priority_queue<int> maxHeap; // Max-heap for the smaller half
    priority_queue<int, vector<int>, greater<int>> minHeap; // Min-heap for the larger half

public:
    // Constructor
    MedianFinder() {
        
    }

    // Function to insert a number into the data structure.
    void addNum(int num) {
        if (maxHeap.empty() || num <= maxHeap.top()) {
            maxHeap.push(num);
        } 
        else {
            minHeap.push(num);
        }
        
        balanceHeaps();
    }

    // Function to balance the heaps.
    void balanceHeaps() {
        if (maxHeap.size() > minHeap.size() + 1) {  // diff greater than 1
            minHeap.push(maxHeap.top());
            maxHeap.pop();
        } 
        else if (minHeap.size() > maxHeap.size()) {
            maxHeap.push(minHeap.top());
            minHeap.pop();
        }
    }

    // Function to return the median.
    double findMedian() {
        if (maxHeap.size() > minHeap.size()) {
            return maxHeap.top();
        } 
        else {
            return (maxHeap.top() + minHeap.top()) / 2.0;
        }
    }
};


/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
```

Let's break down the ETSD (Explanation, Time Complexity, Space Complexity, and Dry Run) for the `MedianFinder` class.

### Explanation:
The `MedianFinder` class is designed to efficiently track and find the median of a stream of integers. The class uses two heaps:
1. **Max-Heap (`maxHeap`)**: This heap stores the smaller half of the numbers.
2. **Min-Heap (`minHeap`)**: This heap stores the larger half of the numbers.

#### Key Operations:
- **`addNum(int num)`**: 
  - Inserts a new number into one of the heaps. If the number is less than or equal to the maximum element in the `maxHeap`, it goes into the `maxHeap`; otherwise, it goes into the `minHeap`.
  - After inserting, the heaps are balanced to ensure that the difference in sizes between `maxHeap` and `minHeap` is at most 1.

- **`balanceHeaps()`**:
  - Balances the heaps if the size difference between them exceeds 1. If `maxHeap` has more elements, the top element of `maxHeap` is moved to `minHeap`. If `minHeap` has more elements, the top element of `minHeap` is moved to `maxHeap`.

- **`findMedian()`**:
  - Returns the median of the current stream of numbers. If `maxHeap` has more elements, the median is the top of `maxHeap`. If both heaps have the same number of elements, the median is the average of the tops of `maxHeap` and `minHeap`.

### Time Complexity:
- **`addNum(int num)`**:
  - Inserting an element into a heap takes \(O(\log n)\) time, where \(n\) is the number of elements in the heap.
  - Balancing the heaps also takes \(O(\log n)\) time in the worst case.
  - **Total Time Complexity:** \(O(\log n)\) per insertion.

- **`findMedian()`**:
  - Retrieving the top element of a heap is \(O(1)\).
  - If both heaps are of equal size, calculating the median involves accessing the top elements of both heaps and computing their average.
  - **Total Time Complexity:** \(O(1)\) per median retrieval.

### Space Complexity:
- The space complexity is dominated by the space required to store the elements in the two heaps.
- **Total Space Complexity:** \(O(n)\), where \(n\) is the number of elements added to the `MedianFinder`.

### Dry Run:
Let's go through a dry run with the following sequence of numbers: `1, 2, 3`.

1. **Initial State:**
   - `maxHeap`: empty
   - `minHeap`: empty

2. **Adding 1 (`addNum(1)`):**
   - Since `maxHeap` is empty, 1 is added to `maxHeap`.
   - `maxHeap`: [1]
   - `minHeap`: []
   - No balancing is required.
   - **Median (`findMedian()`):** 1 (top of `maxHeap`).

3. **Adding 2 (`addNum(2)`):**
   - 2 is greater than the top of `maxHeap` (1), so 2 is added to `minHeap`.
   - `maxHeap`: [1]
   - `minHeap`: [2]
   - No balancing is required as both heaps have the same size.
   - **Median (`findMedian()`):** (1 + 2) / 2 = 1.5.

4. **Adding 3 (`addNum(3)`):**
   - 3 is greater than the top of `maxHeap` (1), so 3 is added to `minHeap`.
   - `maxHeap`: [1]
   - `minHeap`: [2, 3]
   - Balancing is required because `minHeap` has more elements.
   - After balancing, the top element of `minHeap` (2) is moved to `maxHeap`.
   - `maxHeap`: [2, 1]
   - `minHeap`: [3]
   - **Median (`findMedian()`):** 2 (top of `maxHeap`).

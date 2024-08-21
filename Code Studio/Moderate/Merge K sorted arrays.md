```cpp
#include <bits/stdc++.h> 
using namespace std;

vector<int> mergeKSortedArrays(vector<vector<int>>& kArrays, int k) {
    vector<int> vec;
    priority_queue<int, vector<int>, greater<int>> mini;

    // Insert all elements of all arrays into the min-heap
    for (int i=0; i<k; i++) {
        for (int j=0; j<kArrays[i].size(); j++) {
            mini.push(kArrays[i][j]);
        }
    }

    // Extract elements from the min-heap and store them in the result vector
    while (!mini.empty()) {
        int s=mini.top();
        mini.pop();
        vec.push_back(s);
    }

    return vec;
}
```
### Explanation

The given C++ code merges `k` sorted arrays into a single sorted array using a min-heap (or priority queue).

1. **Input Parameters**:
   - `kArrays`: A vector of vectors where each sub-vector is a sorted array.
   - `k`: The number of sorted arrays.

2. **Output**:
   - The function returns a vector containing all elements from the input arrays, sorted in non-decreasing order.

3. **min-heap (priority queue)**:
   - A `priority_queue<int, vector<int>, greater<int>>` is used as a min-heap. This data structure always keeps the smallest element at the top.

4. **Insertion into the Min-Heap**:
   - The code iterates over each element of every array in `kArrays` and inserts each element into the min-heap. This step ensures that all elements are stored in a way that allows for easy retrieval of the smallest element.

5. **Extracting and Storing Sorted Elements**:
   - The smallest element (the top of the min-heap) is repeatedly popped from the heap and added to the result vector `vec`. This process continues until the heap is empty.

6. **Return**:
   - The sorted vector `vec` is returned, containing all the elements from the input arrays in sorted order.

### Time Complexity

- **Heap Insertion**: Inserting an element into a heap takes `O(log n)` time, where `n` is the number of elements in the heap. In this case, there are `N = k * average_size` elements across all arrays.
- **Total Insertion Time**: Inserting all `N` elements into the heap takes `O(N log N)` time.
- **Extracting Elements**: Each extraction operation from the heap also takes `O(log N)` time, and since there are `N` elements, the total extraction time is `O(N log N)`.

Thus, the overall time complexity is `O(N log N)`.

### Space Complexity

- **Heap Space**: The space used by the heap is `O(N)` because all elements are stored in the heap.
- **Result Vector**: The space required for the result vector is `O(N)`.

Therefore, the total space complexity is `O(N)`.

### Dry Run

Let's consider an example:

```cpp
vector<vector<int>> kArrays = {{1, 3, 5}, {2, 4, 6}, {0, 9, 10}};
int k = 3;
```

1. **Initial State**:
   - `mini`: Empty
   - `vec`: Empty

2. **Insert Elements**:
   - Insert all elements into `mini`: [0, 1, 5, 2, 3, 6, 9, 4, 10]
   - The min-heap automatically orders the elements: `mini = [0, 1, 2, 3, 4, 5, 6, 9, 10]`

3. **Extract Elements**:
   - Pop elements one by one and append to `vec`:
   - `vec` after extraction: [0, 1, 2, 3, 4, 5, 6, 9, 10]

4. **Final Output**:
   - The function returns `vec = [0, 1, 2, 3, 4, 5, 6, 9, 10]`, the merged and sorted array.

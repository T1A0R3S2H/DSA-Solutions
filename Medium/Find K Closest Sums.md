# NEW THING LEARNT (Lambda Function)
### Code

```cpp
#include <vector>
#include <algorithm>
#include <cmath>

using namespace std;

class Solution {
public:
    vector<int> findClosestElements(vector<int>& arr, int k, int x) {
        // Sort the array based on the custom comparator
        sort(arr.begin(), arr.end(), [x](int a, int b) {
            return abs(a - x) < abs(b - x) || (abs(a - x) == abs(b - x) && a < b);
        });
        
        // Collect the first k elements
        vector<int> ans(arr.begin(), arr.begin() + k);
        
        // Sort the collected elements before returning
        sort(ans.begin(), ans.end());
        
        return ans;
    }
};
```




### Explanation

1. **Sorting with Comparator**: The array is sorted using a custom comparator. The comparator sorts elements based on their absolute distance from \( x \). If two elements have the same distance from \( x \), they are sorted by their value to ensure stability.

2. **Selecting k Elements**: After sorting, the first \( k \) elements of the array are selected. These elements are the closest to \( x \) according to the custom comparator.

3. **Final Sorting**: The selected \( k \) elements are then sorted in ascending order before being returned.

### Custom Comparator Explanation

The custom comparator is used to define a specific sorting order for the elements in the array based on their distance from \( x \).

```cpp
sort(arr.begin(), arr.end(), [x](int a, int b) {
    return abs(a - x) < abs(b - x) || (abs(a - x) == abs(b - x) && a < b);
});
```

- **Lambda Function**: The lambda function `[x](int a, int b)` is an anonymous function that captures the value of \( x \) from the surrounding scope.
- **Parameters**: The parameters `(int a, int b)` represent two elements from the array that need to be compared.
- **Return Statement**:
  ```cpp
  return abs(a - x) < abs(b - x) || (abs(a - x) == abs(b - x) && a < b);
  ```

  - `abs(a - x) < abs(b - x)`: This checks if element `a` is closer to \( x \) than element `b`.
  - `||`: Logical OR operator.
  - `(abs(a - x) == abs(b - x) && a < b)`: If `a` and `b` are equally close to \( x \), the comparator ensures that the smaller value comes first.

### Why This Comparator?

- The comparator ensures that elements are sorted by their absolute distance to \( x \), which is the primary requirement.
- In case of ties (same distance), it sorts the elements by their natural order (ascending), ensuring a stable sort and correct results when distances are equal.

### Time Complexity (TC)

1. **Sorting**: The initial sort operation takes \( O(n \log n) \) where \( n \) is the number of elements in the array.
2. **Selecting k Elements**: Selecting the first \( k \) elements takes \( O(k) \) time.
3. **Final Sorting**: Sorting the \( k \) elements takes \( O(k \log k) \) time.

Overall time complexity: \( O(n \log n + k \log k) \). Since \( k \) is generally much smaller than \( n \), the dominant term is \( O(n \log n) \).

### Space Complexity (SC)

The space complexity is \( O(k) \) for storing the \( k \) closest elements. No additional space proportional to \( n \) is required, so the overall space complexity is \( O(k) \).

### Dry Run

Let's dry run the example with `arr = [1, 2, 3, 4, 5]`, `k = 4`, and `x = 3`.

1. **Initial Array**: `[1, 2, 3, 4, 5]`
2. **Sorting with Comparator**:
   - Calculate distances from \( x \):
     - `abs(1 - 3) = 2`
     - `abs(2 - 3) = 1`
     - `abs(3 - 3) = 0`
     - `abs(4 - 3) = 1`
     - `abs(5 - 3) = 2`
   - Sort by distance and then by value:
     - Sorted array based on comparator: `[3, 2, 4, 1, 5]`
3. **Selecting k Elements**:
   - First \( k = 4 \) elements: `[3, 2, 4, 1]`
4. **Final Sorting**:
   - Sort the selected elements: `[1, 2, 3, 4]`
5. **Result**: `[1, 2, 3, 4]`


### Summary

The approach first sorts the array based on the distance from \( x \) using a custom comparator, then selects the closest \( k \) elements, and finally sorts these elements to ensure the output is in ascending order. The time complexity is dominated by the sorting step, making it \( O(n \log n) \), and the space complexity is \( O(k) \) for storing the result.

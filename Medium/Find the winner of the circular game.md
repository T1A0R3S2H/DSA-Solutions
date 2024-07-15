# Method 1 (Queues)

### Initial Setup:
- Queue: [1, 2, 3, 4, 5]

### Steps:

1. **First Iteration:**
   - Move \( k-1 = 1 \) friends to the end of the queue:
     - Move 1 to the end: [2, 3, 4, 5, 1]
   - Remove the k-th friend:
     - Remove 2: [3, 4, 5, 1]
   - Next start is friend 3.

2. **Second Iteration:**
   - Move \( k-1 = 1 \) friends to the end of the queue:
     - Move 3 to the end: [4, 5, 1, 3]
   - Remove the k-th friend:
     - Remove 4: [5, 1, 3]
   - Next start is friend 5.

3. **Third Iteration:**
   - Move \( k-1 = 1 \) friends to the end of the queue:
     - Move 5 to the end: [1, 3, 5]
   - Remove the k-th friend:
     - Remove 1: [3, 5]
   - Next start is friend 3.

4. **Fourth Iteration:**
   - Move \( k-1 = 1 \) friends to the end of the queue:
     - Move 3 to the end: [5, 3]
   - Remove the k-th friend:
     - Remove 5: [3]
   - Next start is friend 3.

5. **Final State:**
   - Only friend 3 is left in the queue.

So, the winner is friend 3.

### Dry Run Summary:
1. Queue starts as [1, 2, 3, 4, 5].
2. After removing 2, queue is [3, 4, 5, 1].
3. After removing 4, queue is [5, 1, 3].
4. After removing 1, queue is [3, 5].
5. After removing 5, queue is [3].

The final remaining friend is 3, which matches the expected output.

This step-by-step dry run confirms the correctness of the implementation.

### Corrected Code for Reference:

```cpp
#include <queue>

class Solution {
public:
    int findTheWinner(int n, int k) {
        std::queue<int> q;
        
        // Push friends numbered from 1 to n into the queue
        for(int i = 1; i <= n; i++){
            q.push(i);
        }
        
        // Execute the elimination process until only one friend is left
        while(q.size() > 1) {
            // Move k-1 friends to the end of the queue
            for(int i = 0; i < k - 1; i++) {
                q.push(q.front());
                q.pop();
            }
            // Remove the k-th friend
            q.pop();
        }
        
        // The last remaining friend is the winner
        return q.front();
    }
};
```

## TC: O(n*k), SC=O(n)


# Method 2 (recursion)
Let's do a dry run of the recursive approach for the Josephus problem with the example where \( n = 5 \) and \( k = 2 \).

### Initial Setup:

- We start with \( n = 5 \) and \( k = 2 \).

### Recursive Function Calls:

1. **First Call**: `josephus(5, 2)`
   - This call makes a recursive call to `josephus(4, 2)`.

2. **Second Call**: `josephus(4, 2)`
   - This call makes a recursive call to `josephus(3, 2)`.

3. **Third Call**: `josephus(3, 2)`
   - This call makes a recursive call to `josephus(2, 2)`.

4. **Fourth Call**: `josephus(2, 2)`
   - This call makes a recursive call to `josephus(1, 2)`.

5. **Fifth Call**: `josephus(1, 2)`
   - Base case: returns 0 (only one person).

### Returning from Recursive Calls:

Now we return from each recursive call, calculating the position of the winner step-by-step:

1. **Return from Fifth Call**: `josephus(1, 2)` returns 0.
   - **Fourth Call Calculation**: 
     \[
     \text{josephus}(2, 2) = (\text{josephus}(1, 2) + 2) \% 2 = (0 + 2) \% 2 = 0
     \]
   - **Return from Fourth Call**: `josephus(2, 2)` returns 0.

2. **Return from Fourth Call**: `josephus(2, 2)` returns 0.
   - **Third Call Calculation**: 
     \[
     \text{josephus}(3, 2) = (\text{josephus}(2, 2) + 2) \% 3 = (0 + 2) \% 3 = 2
     \]
   - **Return from Third Call**: `josephus(3, 2)` returns 2.

3. **Return from Third Call**: `josephus(3, 2)` returns 2.
   - **Second Call Calculation**: 
     \[
     \text{josephus}(4, 2) = (\text{josephus}(3, 2) + 2) \% 4 = (2 + 2) \% 4 = 0
     \]
   - **Return from Second Call**: `josephus(4, 2)` returns 0.

4. **Return from Second Call**: `josephus(4, 2)` returns 0.
   - **First Call Calculation**: 
     \[
     \text{josephus}(5, 2) = (\text{josephus}(4, 2) + 2) \% 5 = (0 + 2) \% 5 = 2
     \]
   - **Return from First Call**: `josephus(5, 2)` returns 2.

### Final Step:

- The function `findTheWinner(5, 2)` calls `josephus(5, 2)`, which returns 2.
- Since the Josephus problem uses 0-based indexing and our problem requires 1-based indexing, we adjust the result by adding 1.
- Therefore, the winner is `2 + 1 = 3`.

### Dry Run Summary:
1. Initial call: `josephus(5, 2)`
2. Recursive calls: `josephus(4, 2)`, `josephus(3, 2)`, `josephus(2, 2)`, `josephus(1, 2)`
3. Base case: `josephus(1, 2) = 0`
4. Returning from recursive calls:
   - `josephus(2, 2) = 0`
   - `josephus(3, 2) = 2`
   - `josephus(4, 2) = 0`
   - `josephus(5, 2) = 2`
5. Adjusting to 1-based index: `2 + 1 = 3`

So, the winner is friend 3, which matches the expected output.

### Corrected Code for Reference:

```cpp
class Solution {
public:
    int findTheWinner(int n, int k) {
        return josephus(n, k) + 1; // Adjusting from 0-based index to 1-based index
    }
    
private:
    int josephus(int n, int k) {
        if (n == 1) {
            return 0; // Base case: only one person
        } else {
            return (josephus(n - 1, k) + k) % n; // Recursive case
        }
    }
};
```
## TC: O(n), SC=O(n)

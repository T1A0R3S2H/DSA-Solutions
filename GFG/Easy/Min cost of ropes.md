```cpp
long long minCost(long long arr[], long long n) {
        priority_queue<long long, vector<long long>, greater<long long>> pq;
        for(int i=0; i<n; i++){
            pq.push(arr[i]);
        }
        long long cost=0;
        while(pq.size()>1){
            long long a=pq.top();
            pq.pop();
            long long b=pq.top();
            pq.pop();
            long long sum=a+b;
            cost+=sum;
            pq.push(a+b);
        }
        return cost;
    }
```
### Explanation:
The problem this code solves is about finding the minimum cost of combining elements from an array. The cost of combining two elements is defined as the sum of those two elements, and each combination adds to the total cost. The idea is to always combine the two smallest elements first to minimize the total cost.

Here's how the code works step-by-step:

1. **Priority Queue Initialization**:
   - A `priority_queue` named `pq` is initialized with a `greater<long long>` comparator, making it a min-heap. This ensures that the smallest elements are always at the top.

2. **Inserting Elements into the Priority Queue**:
   - All elements of the array `arr` are inserted into the priority queue. This allows efficient extraction of the two smallest elements.

3. **Combining Elements**:
   - While the priority queue has more than one element:
     - The two smallest elements (`a` and `b`) are extracted from the priority queue.
     - Their sum (`a + b`) is added to the running total `cost`.
     - The sum is then pushed back into the priority queue, so it can be combined with the next smallest elements in subsequent iterations.

4. **Return the Total Cost**:
   - Once the priority queue is reduced to a single element, the total cost of all combinations is returned.

### Time Complexity Analysis:
- **Building the Priority Queue**: Inserting `n` elements into a priority queue takes \(O(n \log n)\) time.
- **While Loop**: The loop runs `n-1` times (since in each iteration, we reduce the size of the priority queue by one). Each iteration involves extracting two elements (each extraction takes \(O(\log n)\)) and inserting their sum back (which also takes \(O(\log n)\)). Hence, the while loop takes \(O(n \log n)\) time.
- **Overall Time Complexity**: The total time complexity is \(O(n \log n)\).

### Space Complexity Analysis:
- **Priority Queue Storage**: The priority queue stores `n` elements, so it requires \(O(n)\) space.
- **Additional Variables**: The space required for variables like `cost`, `a`, `b`, and `sum` is \(O(1)\).
- **Overall Space Complexity**: The total space complexity is \(O(n)\).

### Dry Run:
Let's go through a dry run with an example input.

#### Input:
``` 
n = 4
arr = [4, 3, 2, 6]
```

#### Steps:
1. **Initialization**: 
   - Priority Queue: `pq = []`
   - Insert elements into `pq`: 
     - `pq = [2, 3, 4, 6]`
   - `cost = 0`

2. **First Iteration**:
   - Extract `a = 2` and `b = 3`.
   - `sum = 2 + 3 = 5`
   - Add `sum` to `cost`: `cost = 0 + 5 = 5`
   - Insert `5` back into `pq`: `pq = [4, 5, 6]`

3. **Second Iteration**:
   - Extract `a = 4` and `b = 5`.
   - `sum = 4 + 5 = 9`
   - Add `sum` to `cost`: `cost = 5 + 9 = 14`
   - Insert `9` back into `pq`: `pq = [6, 9]`

4. **Third Iteration**:
   - Extract `a = 6` and `b = 9`.
   - `sum = 6 + 9 = 15`
   - Add `sum` to `cost`: `cost = 14 + 15 = 29`
   - Insert `15` back into `pq`: `pq = [15]`

5. **Termination**:
   - The priority queue now has only one element, so the loop exits.
   - The final `cost` is `29`.

#### Output:
```
29
```

This matches the expected output, confirming the correctness of the code.

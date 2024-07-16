# Method 1 (deque+sliding window)
```cpp
vector<long long> printFirstNegativeInteger(long long int A[], long long int N, long long int K) {
    
        deque<long long int> dq;
        vector<long long> ans;
        
        //push k window sized elements, if they are -ve to the deque
        for (int i=0;i<K;i++){
            if (A[i]<0){
                dq.push_back(i);
            }
        }
        
        //form the o/p array
        if (!dq.empty()){
            ans.push_back(A[dq.front()]);
        }
        else ans.push_back(0);
        
        
        for (int i=K;i<N;i++){
            while (!dq.empty() and i-dq.front()>K-1){
                dq.pop_front();
            }
            
            if (A[i]<0) {
                dq.push_back(i);
            }
            if (!dq.empty()) {
                ans.push_back(A[dq.front()]);
            }
            else {
                ans.push_back(0);
            }
        }
        return ans;
 }
```
## Code Explanation:


1. **Initialization:**
    - `deque<long long int> dq;` : This deque (double-ended queue) will store the indices of negative numbers in the current window.
    - `vector<long long> ans;` : This vector will store the final result of first negative integers for each window.

2. **Processing the first window of size K:**
    - The for loop `for (int i=0;i<K;i++)` iterates through the first K elements of the array.
    - If an element `A[i]` is negative, its index `i` is pushed into the deque.

3. **Setting the initial result for the first window:**
    - After the first window is processed, if the deque is not empty, it means there is at least one negative number in the first window, and the element at the front of the deque is the first negative number. This value is added to the result vector `ans`.
    - If the deque is empty, it means there are no negative numbers in the first window, so 0 is added to the result vector `ans`.

4. **Processing subsequent windows:**
    - The for loop `for (int i=K;i<N;i++)` iterates through the rest of the array, starting from the (K+1)th element.
    - Before processing each new element, we remove indices from the front of the deque that are out of the current window by checking `i-dq.front()>K-1`.
    - If the current element `A[i]` is negative, its index `i` is pushed into the deque.
    - After updating the deque for the current window, the first element in the deque is the first negative number of the current window (if the deque is not empty), and it is added to the result vector `ans`. If the deque is empty, 0 is added to `ans`.

5. **Returning the result:**
    - The function returns the result vector `ans`.
  


## Steps Explained
The reason for processing the first window separately in the function is to handle the initial setup for the sliding window approach. Let's break down the process to understand this better.

1. **Initialize Data Structures:**
   - `deque<long long int> dq`: This will store indices of negative numbers in the current window.
   - `vector<long long> ans`: This will store the first negative integer for each window of size \( K \).

2. **Processing the First Window (Initial Setup):**
   ```cpp
   for (int i = 0; i < K; i++) {
       if (A[i] < 0) {
           dq.push_back(i);
       }
   }
   ```
   - This loop processes the first \( K \) elements (i.e., the first window). If an element is negative, its index is pushed into the deque.

3. **Forming the Output for the First Window:**
   ```cpp
   if (!dq.empty()) {
       ans.push_back(A[dq.front()]);
   } else {
       ans.push_back(0);
   }
   ```
   - After processing the first \( K \) elements, the function checks if there are any negative numbers in the deque. If the deque is not empty, it means there is at least one negative number in the current window, and the function adds the first negative number (indexed by `dq.front()`) to the output. If the deque is empty, it means there are no negative numbers in the current window, so the function adds `0` to the output.

4. **Sliding the Window:**
   ```cpp
   for (int i = K; i < N; i++) {
       while (!dq.empty() and i - dq.front() > K - 1) {
           dq.pop_front();
       }
       
       if (A[i] < 0) {
           dq.push_back(i);
       }
       if (!dq.empty()) {
           ans.push_back(A[dq.front()]);
       } else {
           ans.push_back(0);
       }
   }
   ```
   - This loop processes the rest of the elements, one by one, sliding the window to the right by one position at a time.
   - **Remove Out of Bound Indices:** The inner `while` loop ensures that indices in the deque which are out of the current window are removed (`i - dq.front() > K - 1`).
   - **Add New Element:** If the current element (`A[i]`) is negative, its index is added to the deque.
   - **Form Output for Current Window:** Similar to the first window, if the deque is not empty, the first negative number in the current window is added to the output. Otherwise, `0` is added.

### Summary

- **Initial Window Setup:** The first window is processed separately to initialize the deque with indices of negative numbers in the first \( K \) elements.
- **Sliding Window:** After processing the first window, the sliding window logic takes over, where the window slides one element to the right at a time, updating the deque and forming the output accordingly.

Processing the first window separately simplifies the logic and ensures that the initial state is correctly set up for the sliding window process.

### Dry Run:

Let's dry run the given code with the example input:

#### Example 1:
Input: 
N = 5
A[] = {-8, 2, 3, -6, 10}
K = 2

**Initialization:**
- `dq = []`
- `ans = []`

**Processing the first window (K=2):**
- For i=0: A[0] = -8 is negative, so dq = [0]
- For i=1: A[1] = 2 is not negative, so dq remains [0]
- First window: `dq` is not empty, so ans = [-8]

**Processing subsequent windows:**
- For i=2: 
  - Before processing: dq = [0]
  - Check indices out of current window: 2-0 > 1, so pop_front(), dq = []
  - A[2] = 3 is not negative, so dq remains []
  - Current window: `dq` is empty, so ans = [-8, 0]

- For i=3:
  - Before processing: dq = []
  - A[3] = -6 is negative, so dq = [3]
  - Current window: `dq` is not empty, so ans = [-8, 0, -6]

- For i=4:
  - Before processing: dq = [3]
  - Check indices out of current window: 4-3 > 1, so pop_front(), dq = []
  - A[4] = 10 is not negative, so dq remains []
  - Current window: `dq` is empty, so ans = [-8, 0, -6, -6]

**Result:**
- The function returns `[-8, 0, -6, -6]`

#### Example 2:
Input: 
N = 8
A[] = {12, -1, -7, 8, -15, 30, 16, 28}
K = 3

**Initialization:**
- `dq = []`
- `ans = []`

**Processing the first window (K=3):**
- For i=0: A[0] = 12 is not negative, so dq remains []
- For i=1: A[1] = -1 is negative, so dq = [1]
- For i=2: A[2] = -7 is negative, so dq = [1, 2]
- First window: `dq` is not empty, so ans = [-1]

**Processing subsequent windows:**
- For i=3: 
  - Before processing: dq = [1, 2]
  - Check indices out of current window: 3-1 > 2, no pop needed
  - A[3] = 8 is not negative, so dq remains [1, 2]
  - Current window: `dq` is not empty, so ans = [-1, -1]

- For i=4:
  - Before processing: dq = [1, 2]
  - Check indices out of current window: 4-1 > 2, so pop_front(), dq = [2]
  - A[4] = -15 is negative, so dq = [2, 4]
  - Current window: `dq` is not empty, so ans = [-1, -1, -7]

- For i=5:
  - Before processing: dq = [2, 4]
  - Check indices out of current window: 5-2 > 2, so pop_front(), dq = [4]
  - A[5] = 30 is not negative, so dq remains [4]
  - Current window: `dq` is not empty, so ans = [-1, -1, -7, -15]

- For i=6:
  - Before processing: dq = [4]
  - Check indices out of current window: 6-4 > 2, no pop needed
  - A[6] = 16 is not negative, so dq remains [4]
  - Current window: `dq` is not empty, so ans = [-1, -1, -7, -15, -15]

- For i=7:
  - Before processing: dq = [4]
  - Check indices out of current window: 7-4 > 2, so pop_front(), dq = []
  - A[7] = 28 is not negative, so dq remains []
  - Current window: `dq` is empty, so ans = [-1, -1, -7, -15, -15, 0]

**Result:**
- The function returns `[-1, -1, -7, -15, -15, 0]`

The code correctly finds the first negative integer in every window of size K in the array.


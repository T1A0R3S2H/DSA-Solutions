# Method 1 (brute force, O(n^2))
```cpp
public:
    //Function to find the next greater element for each element of the array.
    vector<long long> nextLargerElement(vector<long long> arr, int n){
        // Your code here
        vector<long long> ans(n,-1); //value to be strored in n or default value -1
        for (int i = 0; i < n; i++) {
            for (int j =i+1; j < n; j++) {
                if (arr[j] > arr[i]) {
                    ans[i] = arr[j];
                    break;
                }
            }
        }
        return ans; //it will return array of data stored in ans if a[j]>a[i] with -1 if a[j]<=a[i]
    }
```
![image](https://github.com/T1A0R3S2H/Leetcode-Progess/assets/123285559/df0056f4-8fae-4989-a64d-5caa924e1c5d)


# Method 2 (stacks)

### Approach Explanation

The provided code finds the next larger element for each element in the array `arr`. The next larger element for an element `arr[i]` is the first element to the right of `arr[i]` that is larger than `arr[i]`. If no such element exists, `-1` is returned for that position.

Here's the step-by-step explanation of the approach:

1. **Initialize Stack**: A stack is initialized with a single element `-1`. This `-1` acts as a sentinel value to handle cases where there is no larger element to the right of an array element.

2. **Iterate from Right to Left**: Loop through the array `arr` from the last element to the first element (right to left).

3. **Pop Elements from Stack**: For the current element `arr[i]`, pop elements from the stack until the top of the stack is greater than `arr[i]`. This ensures that the top of the stack is the next larger element for `arr[i]`.

4. **Assign Next Larger Element**: Assign the top of the stack as the next larger element for `arr[i]`.

5. **Push Current Element to Stack**: Push the current element `arr[i]` onto the stack for future comparisons.

6. **Return the Modified Array**: After processing all elements, return the modified array which now contains the next larger elements.

### Dry Run

Let's dry run the algorithm with an example array `arr = [4, 5, 2, 10, 8]`.

1. **Initialization**:
    - Stack: `[-1]`
    - Input array: `[4, 5, 2, 10, 8]`

2. **Iteration** (from right to left):

    - **i = 4 (arr[4] = 8)**:
        - Stack: `[-1]`
        - No elements in stack are greater than `8`.
        - Next larger element for `8` is `-1` (top of the stack).
        - Updated array: `[4, 5, 2, 10, -1]`
        - Push `8` onto the stack.
        - Stack: `[-1, 8]`

    - **i = 3 (arr[3] = 10)**:
        - Stack: `[-1, 8]`
        - Top of the stack `8` is less than `10`, pop it.
        - Stack: `[-1]`
        - No elements in stack are greater than `10`.
        - Next larger element for `10` is `-1` (top of the stack).
        - Updated array: `[4, 5, 2, -1, -1]`
        - Push `10` onto the stack.
        - Stack: `[-1, 10]`

    - **i = 2 (arr[2] = 2)**:
        - Stack: `[-1, 10]`
        - Top of the stack `10` is greater than `2`.
        - Next larger element for `2` is `10` (top of the stack).
        - Updated array: `[4, 5, 10, -1, -1]`
        - Push `2` onto the stack.
        - Stack: `[-1, 10, 2]`

    - **i = 1 (arr[1] = 5)**:
        - Stack: `[-1, 10, 2]`
        - Top of the stack `2` is less than `5`, pop it.
        - Stack: `[-1, 10]`
        - Top of the stack `10` is greater than `5`.
        - Next larger element for `5` is `10` (top of the stack).
        - Updated array: `[4, 10, 10, -1, -1]`
        - Push `5` onto the stack.
        - Stack: `[-1, 10, 5]`

    - **i = 0 (arr[0] = 4)**:
        - Stack: `[-1, 10, 5]`
        - Top of the stack `5` is greater than `4`.
        - Next larger element for `4` is `5` (top of the stack).
        - Updated array: `[5, 10, 10, -1, -1]`
        - Push `4` onto the stack.
        - Stack: `[-1, 10, 5, 4]`

3. **Final Output**:
    - `[5, 10, 10, -1, -1]`

### Code with Comments

```cpp
vector<long long> nextLargerElement(vector<long long> arr, int n) {
    // Initialize a stack and push -1 to handle cases where no larger element exists
    stack<long long> st;
    st.push(-1);
    
    // Iterate from right to left
    for(int i = n - 1; i >= 0; i--) {
        // Pop elements from the stack until the top is greater than the current element
        while(st.size() != 1) {
            if(st.top() > arr[i])
                break;
            st.pop();
        }
        // Assign the next larger element
        long long t = arr[i];
        arr[i] = st.top();
        // Push the current element onto the stack
        st.push(t);
    }
    // Return the modified array
    return arr;
}
```

This approach ensures that each element is processed efficiently, leading to an overall time complexity of O(n).

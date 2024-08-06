```cpp
struct petrolPump {
    int petrol;
    int distance;
};

int tour(petrolPump p[], int n) {
    int total_petrol = 0;  // Total petrol balance
    int current_petrol = 0;  // Current petrol balance
    int start_index = 0;  // Starting petrol pump index

    // Traverse each petrol pump
    for (int i = 0; i < n; i++) {
        total_petrol += p[i].petrol - p[i].distance;
        current_petrol += p[i].petrol - p[i].distance;

        // If current petrol balance is negative, update the starting index
        if (current_petrol < 0) {
            start_index = i + 1;
            current_petrol = 0;
        }
    }

    // Check if the total petrol balance is non-negative
    if (total_petrol >= 0) {
        return start_index;
    } else {
        return -1;
    }
}
```

### Explanation:
- **total_petrol**: Tracks the net balance of petrol after completing the circle.
- **current_petrol**: Tracks the net balance of petrol for the current segment of the journey.
- **start_index**: Marks the potential starting point for the journey.

#### Steps:
1. Iterate through each petrol pump.
2. Update the `total_petrol` and `current_petrol` balances.
3. If `current_petrol` drops below zero, it means the current starting point cannot work, so we move the starting point to the next petrol pump (`i + 1`) and reset `current_petrol`.
4. After the loop, check if `total_petrol` is non-negative, which indicates a valid starting point exists. If yes, return `start_index`; otherwise, return `-1`.

This approach ensures that we find the first valid starting point in O(N) time complexity and O(1) auxiliary space, as required.

# DRY RUN
Let's do a dry run of the given code using the example input:

- **N = 4**
- **Petrol = [4, 6, 7, 4]**
- **Distance = [6, 5, 3, 5]**

These can be represented as petrol pumps:
- {4, 6}, {6, 5}, {7, 3}, {4, 5}

### Dry Run:

1. **Initialize variables**:
   ```cpp
   total_petrol = 0;
   current_petrol = 0;
   start_index = 0;
   ```

2. **Iteration 1 (i = 0)**:
   - Petrol at pump 0: 4
   - Distance to next pump: 6
   - Petrol balance: 4 - 6 = -2
   - Update variables:
     ```cpp
     total_petrol += -2;    // total_petrol = -2
     current_petrol += -2;  // current_petrol = -2
     ```
   - Since `current_petrol` < 0:
     - Update `start_index` to 1
     - Reset `current_petrol` to 0
     ```cpp
     start_index = 1;
     current_petrol = 0;
     ```

3. **Iteration 2 (i = 1)**:
   - Petrol at pump 1: 6
   - Distance to next pump: 5
   - Petrol balance: 6 - 5 = 1
   - Update variables:
     ```cpp
     total_petrol += 1;    // total_petrol = -1
     current_petrol += 1;  // current_petrol = 1
     ```

4. **Iteration 3 (i = 2)**:
   - Petrol at pump 2: 7
   - Distance to next pump: 3
   - Petrol balance: 7 - 3 = 4
   - Update variables:
     ```cpp
     total_petrol += 4;    // total_petrol = 3
     current_petrol += 4;  // current_petrol = 5
     ```

5. **Iteration 4 (i = 3)**:
   - Petrol at pump 3: 4
   - Distance to next pump: 5
   - Petrol balance: 4 - 5 = -1
   - Update variables:
     ```cpp
     total_petrol += -1;    // total_petrol = 2
     current_petrol += -1;  // current_petrol = 4
     ```

6. **Final Check**:
   - After the loop, check if `total_petrol` is non-negative:
     ```cpp
     if (total_petrol >= 0) {
         return start_index;
     } else {
         return -1;
     }
     ```
   - Since `total_petrol = 2` (which is non-negative), return `start_index`:
     ```cpp
     return 1;
     ```

### Conclusion:

The truck can start from petrol pump index 1 (second petrol pump) to complete the circle. Therefore, the output is 1.

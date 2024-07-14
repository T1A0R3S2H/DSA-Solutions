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

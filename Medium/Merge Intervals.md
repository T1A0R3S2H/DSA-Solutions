## Method 1
```cpp
int n = arr.size(); // size of the array

    //sort the given intervals:
    sort(arr.begin(), arr.end());

    vector<vector<int>> ans;

    for (int i = 0; i < n; i++) {
        // if the current interval does not
        // lie in the last interval:
        if (ans.empty() || arr[i][0] > ans.back()[1]) {
            ans.push_back(arr[i]);
        }
        // if the current interval
        // lies in the last interval:
        else {
            ans.back()[1] = max(ans.back()[1], arr[i][1]);
        }
    }
    return ans;
```

Let's do a dry run of the provided code using the given input:

**Input:**
```
Intervals = {{1,3},{2,4},{6,8},{9,10}}
```

**Output:**
```
{{1, 4}, {6, 8}, {9, 10}}
```

### Steps:

1. **Initialization and Sorting:**
   ```cpp
   int n = intervals.size(); // n = 4
   sort(intervals.begin(), intervals.end()); // intervals = {{1,3},{2,4},{6,8},{9,10}}
   vector<vector<int>> ans;
   ```

2. **First Iteration (i = 0):**
   ```cpp
   if (ans.empty() || intervals[i][0] > ans.back()[1]) {
       ans.push_back(intervals[i]); // ans = {{1,3}}
   }
   ```

3. **Second Iteration (i = 1):**
   ```cpp
   if (ans.empty() || intervals[i][0] > ans.back()[1]) {
       // Not executed, since intervals[1][0] = 2 is not greater than ans.back()[1] = 3
   } else {
       ans.back()[1] = max(ans.back()[1], intervals[i][1]); // ans.back()[1] = max(3, 4) = 4
   }
   // ans = {{1,4}}
   ```

4. **Third Iteration (i = 2):**
   ```cpp
   if (ans.empty() || intervals[i][0] > ans.back()[1]) {
       ans.push_back(intervals[i]); // ans = {{1,4}, {6,8}}
   }
   ```

5. **Fourth Iteration (i = 3):**
   ```cpp
   if (ans.empty() || intervals[i][0] > ans.back()[1]) {
       ans.push_back(intervals[i]); // ans = {{1,4}, {6,8}, {9,10}}
   }
   ```

### Final Output:
The final merged intervals stored in `ans` are:
```
{{1, 4}, {6, 8}, {9, 10}}
```

This matches the expected output.

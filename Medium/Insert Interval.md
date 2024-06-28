# Method 1 (2 pointers)

## Problem Statement

Given a set of non-overlapping intervals sorted in ascending order by start time, insert a new interval into the set. Merge overlapping intervals if necessary.

## Function Signature

```cpp
vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval)
```

### Parameters
- `intervals`: A vector of vectors representing sorted, non-overlapping intervals.
- `newInterval`: A vector representing the new interval to be inserted.

### Return Value
- A vector of vectors representing the merged intervals after insertion.

## Algorithm Explanation

1. **Initialization**
   - Create an empty vector `mergedIntervals` to store the result.
   - Extract the start and end points of the new interval.

2. **Iteration**
   Iterate through each interval in the input `intervals`:

   a. **Case 1: Current interval is after the new interval**
      - If the current interval starts after the new interval ends:
        - Add the new interval to `mergedIntervals`.
        - Update `newStart` and `newEnd` to the current interval.

   b. **Case 2: Current interval is before the new interval**
      - If the new interval starts after the current interval ends:
        - Add the current interval to `mergedIntervals`.

   c. **Case 3: Intervals overlap**
      - If there's an overlap:
        - Update `newStart` to the minimum of `newStart` and the current interval's start.
        - Update `newEnd` to the maximum of `newEnd` and the current interval's end.

3. **Final Step**
   - After the iteration, add the last interval (which might be merged) to `mergedIntervals`.

4. **Return**
   - Return the `mergedIntervals` vector.

## Time Complexity
- O(n), where n is the number of intervals in the input.

## Space Complexity
- O(n) in the worst case, where no merging occurs and we end up with n+1 intervals.

## Code
```cpp
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, 
                               vector<int>& newInterval) {
        vector<vector<int>> mergedIntervals;
        int newStart = newInterval[0], newEnd = newInterval[1];

        for (const auto& interval : intervals) {
            int currentStart = interval[0], currentEnd = interval[1];

            if (currentStart > newEnd) {
                mergedIntervals.push_back({newStart, newEnd});
                newStart = currentStart;
                newEnd = currentEnd;
            } 
            else if (newStart > currentEnd) {
                mergedIntervals.push_back(interval);
            } 
            else { // overlapping intervals
                newStart = min(newStart, currentStart);
                newEnd = max(newEnd, currentEnd);
            }
        }

        mergedIntervals.push_back({newStart, newEnd});

        return mergedIntervals;
    }
};
```

## Example

```cpp
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]

Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
```

## Notes
- The algorithm assumes that the input intervals are already sorted by start time.
- It handles all cases of interval relationships: non-overlapping, partially overlapping, and fully contained intervals.

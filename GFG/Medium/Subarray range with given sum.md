
```cpp
int subArraySum(vector<int>& arr, int tar) {
        int sum=0;
        int count=0;
        unordered_map<int,int> mp;
        for(int i=0; i<arr.size(); i++){
            sum+=arr[i];

            //case 1: agar directly mil gaya
            if(sum==tar) count++;

            //case 2: complement of the required sum
            if(mp.find(sum-tar)!=mp.end()){
                count+=mp[sum-tar];
            }
            mp[sum]++;
        }
        return count;
    }
```
---
mp.find(sum - tar) != mp.end(): This checks if the key sum - tar exists in the map mp. If it exists, it means there is a previously seen sum that, when added to tar, equals the current sum.
---

In other words, this is checking if there is a subarray (or subsequence, depending on the problem) that sums to tar.
// Explanation:
// This function finds the number of subarrays in 'arr' that sum up to the target 'tar'.
// It uses a cumulative sum approach with a hash map to efficiently track subarrays.

// 1. Initialize variables:
//    - 'sum' keeps track of the cumulative sum
//    - 'count' stores the number of subarrays found
//    - 'mp' is an unordered map to store frequency of cumulative sums

// 2. Iterate through the array:
//    a. Add current element to 'sum'
//    b. If 'sum' equals 'tar', increment 'count' (found a subarray from start)
//    c. Check if (sum - tar) exists in the map. If it does, add its frequency to 'count'
//    d. Increment the frequency of the current sum in the map

// 3. Return the total count of subarrays

// Time Complexity: O(n)
// - The algorithm iterates through the array once
// - All operations within the loop (map lookup, insertion) are O(1) on average

// Space Complexity: O(n)
// - In the worst case, the unordered_map 'mp' could store n distinct sums
// - Other variables use constant space

```
```
Key points about the algorithm:

1. Cumulative Sum: It uses a running sum to efficiently calculate subarray sums without nested loops.

2. Hash Map Usage: The unordered_map stores frequencies of cumulative sums, allowing quick lookups to find complementary sums.

3. Subarray Identification: When (sum - tar) is found in the map, it means we've discovered one or more subarrays ending at the current index that sum to tar.

4. Handling Zero-Sum Subarrays: The check `if(sum == tar) count++` handles cases where subarrays start from the beginning of the array.

Time Complexity:
- O(n), where n is the size of the input array.
- The algorithm makes a single pass through the array.
- Map operations (insertion, lookup) are O(1) on average.

Space Complexity:
- O(n) in the worst case.
- The unordered_map could potentially store up to n distinct cumulative sums.
- Other variables use constant space.

This algorithm is efficient for large arrays and provides a significant improvement over the naive O(n^2) approach of checking all possible subarrays.

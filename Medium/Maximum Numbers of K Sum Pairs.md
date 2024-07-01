## Method 1 (brute force)
![image](https://github.com/T1A0R3S2H/Leetcode-Progess/assets/123285559/24cf9c57-e6bb-4d8c-84a7-e32ce4fd773b)

```cpp
class Solution {
public:
    int maxOperations(vector<int>& nums, int k) {
        int n=nums.size();
        int left=0;
        int right=n-1;
        int operations=0;
        
        sort(nums.begin(), nums.end());  // Sort the array first
        
        while (left<right) {
            int sum=nums[left]+nums[right];
            if (sum==k) {
                operations++;
                left++;
                right--;
            } 
            else if (sum<k) {
                left++;
            } 
            else {
                right--;
            }
        }
        
        return operations;
    }
};
```
![image](https://github.com/T1A0R3S2H/Leetcode-Progess/assets/123285559/6a9105aa-ab8a-41a0-a949-de0428ec9acc)

## Method 2 (unordered map)
We use an unordered_map called `freq` to store the frequency of each number in the array.
We iterate through the array once, processing each number:

 - For each number, we calculate its `complement (k - num)`.
 - If the complement exists in our frequency map, we've found a pair that `sums to k`.
 - We increment the operations count and decrease the frequency of the complement.
 - If the complement doesn't exist (or its frequency is 0), we increment the frequency of the current number.


At the end, we return the total number of operations.

This approach has several advantages:

It runs in O(n) time complexity, as we only need to iterate through the array once.
It uses O(n) space in the worst case, which is generally better than sorting the entire array.
It doesn't modify the original array, which might be preferred in some scenarios.

This solution efficiently solves the problem without sorting, addressing your concern while improving the overall performance of the algorithm.
```cpp
class Solution {
public:
    int maxOperations(vector<int>& nums, int k) {
        unordered_map<int, int> freq;
        int operations=0;
        
        for (int num:nums) {
            int complement=k-num;
            if (freq[complement]>0) {
                operations++;
                freq[complement]--;
            } 
            else {
                freq[num]++;
            }
        }
        
        return operations;
    }
};
```

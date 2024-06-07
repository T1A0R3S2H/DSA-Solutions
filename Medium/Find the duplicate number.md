## Method 1 (brute force)
```bash
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int n = nums.size();
        sort(nums.begin(), nums.end());
        for (int i = 0; i < n - 1; i++) {
            if (nums[i] == nums[i + 1]) {
                return nums[i];
            }
        }
        // If no duplicate is found, return -1 or any other appropriate value
        return -1;
    }
};
```
## Method 2 (Striver's/ freq map)
```bash
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int n = nums.size();
        vector<int> freq(n + 1, 0); // Initialize a frequency vector of size n+1 with all elements set to 0
        for (int i = 0; i < n; i++) {
            if (freq[nums[i]] == 0) {
                freq[nums[i]]++;
            } else {
                return nums[i];
            }
        }
        return 0;
    }
};
```

## Method 3 (Linked List type approach, Tortoise Hare approach)
?????????????????????????????????
?????????????????????????????????

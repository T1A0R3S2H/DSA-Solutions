# Method 1 (sort)
```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        return nums[nums.size()-k];
    }
};


//sort and return the kth element from the end
```


# Method 2 (priority queue | heap)
```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int> pq(nums.begin(), nums.end());
        for (int i=0; i<k-1; i++) {
            pq.pop();
        }
        return pq.top();
    }
};
```

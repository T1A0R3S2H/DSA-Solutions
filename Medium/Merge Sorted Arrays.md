## Method 1 (in-built function)
```bash
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        nums1.resize(m);
        nums2.resize(n);
        nums1.insert(nums1.end(), nums2.begin(), nums2.end());
        sort(nums1.begin(), nums1.end());
    }
};
```
![image](https://github.com/T1A0R3S2H/Leetcode-Progess/assets/123285559/4f770b0c-4b39-4c0c-beea-9e7daa7d23da)

```cpp
class Solution {
public:
    int averageValue(vector<int>& nums) {
        vector <int> store;
        int ans=0;
        for (int i=0;i<nums.size();i++){
            if (nums[i]%6==0){
                store.push_back(nums[i]);
            }
        }
        for (int i=0;i<store.size();i++){
            ans+=store[i];
        }
        if (store.size() == 0) {
            return 0;
        }
        return (ans/store.size());
    }
};
```

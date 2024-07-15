```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int n=s.size();
        if (n==0) return 0;
        
        int maxi=0;
        unordered_set<char> set;
        int i=0, j=0;
        
        while (i<n && j<n) {
            if (set.find(s[j])==set.end()) {
                set.insert(s[j++]);
                maxi=max(maxi, j-i);
            } 
            else {
                set.erase(s[i++]);
            }
        }
        
        return maxi;
    }
};

```

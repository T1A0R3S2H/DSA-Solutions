```cpp
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        unordered_map<char, int> freq;

        // Iterate through the magazine and count characters
        for (char c : magazine) {
            if (freq.find(c) == freq.end()) {
                freq[c] = 1;
            } 
            else {
                freq[c]++;
            }
        }

        // Iterate through the ransom note and check character counts
        for (char c : ransomNote) {
            if (freq.find(c) != freq.end() && freq[c] > 0) {
                freq[c]--;
            } 
            else {
                return false;
            }
        }

        return true;
    }
};
```

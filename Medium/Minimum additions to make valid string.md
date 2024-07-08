# Method 1 (without stacks)
```cpp
class Solution {
public:
    int addMinimum(string word) {
        string t = "abc";
        int additions = 0;
        int i = 0;
        int j = 0;

        while (i < word.length()) {
            if (word[i] == t[j]) {
                i++;
            } else {
                additions++;
            }
            j = (j + 1) % 3;
        }

        additions += (3 - j) % 3;
        return additions;
    }
};
```
![image](https://github.com/T1A0R3S2H/Leetcode-Progess/assets/123285559/de8a45a9-6fde-40df-92d9-511bb5ddec6c)

# Method 2 (stacks)

